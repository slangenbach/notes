# Snowflake SnowPro Core

Notes on the [course][1] and the [exam prep][2] from Udemy.

## General

- Snowflake offers Standard, Enterprise, Business Critical and Virtual Private editions
- Enterprise offers multi-cluster warehouses, 90 day time travel, materialized views, search optimization, data masking, etc.
- Business Critical offers custom-manager encryption
- Virtual Private edition is made up of a dedicated services and needs to be setup on a by case basis
- We can use BI tools like PowerBI and Tableau to analyze data in Snowflake
- Additionally we can integrate other tools with Snowflake via partner connect
- Finally the Snowflake Marketplace offers access to dataset and apps from third parties
- The Snowflake UI is referred to as *Snowsight*
- The Snowflake CLI is referred to as [SnowSQL][3]. SnowSQL does support scripting for procedural code via an extension
- Snowpark enables different programming language (e.g. Python) to push down code (as SQL) to Snowflake

## Architecture

- Snowflake separates storage, compute (called _warehouse_ in Snowflake lingo) and additional (cloud) services
- Snowflake is built as a multi-cluster (MPP compute cluster) shared data system (compressed columnar storage as BLOB)
- Warehouses sizes range from XS (1 server) to 6-XL (128 servers)
- Data is stored in BLOB storage as hybrid columnar storage
- Warehouses can be scaled horizontally using _standard_ and _economy_ policies. _Standard_ policy focuses on minimize queuing of queries (starts new cluster if when its needed), while _economy_ focuses on preserving credits (starts new cluster only if loaded for approx. 6min)
- Multi-cluster warehouses help especially with many concurrent users

## Pricing

- Snowflake charges for compute, storage and data transfer. Compute and storage can be scaled individually. Charging is done via credits and varies by cloud provider and region
- Compute charges for active warehouses (billed by second, charged per hour), cloud services and other internal services
- Storage charges for storage used in TB (billed monthly, charged by average storage used per month). Users can save on storage costs by choosing capacity-based charging over on-demand based charging.
- Data ingestion is free, egress incurs is charged for. Costs vary by cloud provider and region. Inter-region and inter-cloud-provider transfer is especially costly.
- Costs can be monitored via resource monitors

## Identity and Access Management

- Snowflake combines discretionary access control (DAC) and role-based access control (RBAC)
- DAC means that each object has an owner who can grant access to it
- RBAC means privileges are assigned to roles which are assigned to users
- Securable objects include account, user, role, database (including schema, table, etc.), warehouse, etc.
- The role hierarchy is: 
    - Org Admin (not a role strictly speaking)
        - Account Admin
            - Security Admin > User Admin > Public
            - Sys Admin > Custom Roles
- The account admin can be secured via MFA
- Use the security admin to manage object grants globally
- Use the sys admin to create and manage objects (warehouses, databases, tables) globally
- The sys admin should be assigned any custom role as parent
- Use the user admin to create users and roles, but not to grant privileges
- The public role is attached to every user

## Data Ingestion

- Data can be ingested in bulk or continuously
- A stage in Snowflake refers to an object where data metadata is stored, e.g. its location and the files it contains
- External stages refer to object storage of a cloud provider, e.g. AWS S3 bucket
- Internal stages refer to Snowflake-managed local file storage
- Internally USER, TABLE and NAMED INTERNAL and EXTERNAL stages exist
    - USER stages are tied to users, can not be accessed by other users and not be altered or dropped
    - USER stages can be referred to via `@~`
    - Data from USER stages can be loaded into multipe tables
    - TABLE stages are automatically created with a table
    - TABLE stages can only be accessed by one table
    - TABLE stages can be referred to via `@%<TABLE_STAGE_NAME>`
- We refer to internal and external stages using @STAGE_NAME
- Use the `PUT` command in SnowSQL to upload files to stages and the `GET` command to download (unload) files from *internal* stages

### Ingesting data using the COPY command

- Data can be transformed during loading into stages by using SQL syntax (and certain SQL functions) within the `FROM` clause of the `COPY INTO` command
- We can also only load data into certain columns when copying data into a stage
- We can handle errors during data ingestion using the `ON_ERROR` keyword (which is a copy option). Options include _ABORT_STATEMENT_, _SKIP_FILE_ (we can also specify the threshold of errors at which we skip a file using SKIP_FILE_<THRESHOLD>) and _CONTINUE_ 
- Stages and file formats can be saved into dedicated objects within dedicated schemas within a dedicated databases (e.g. MANAGE_DB)
- Use the copy option `VALIDATION_MODE = RETURN_ERRORS | RETURN_N_ROWS` to validate data without loading it
- We can _work_ with errors by creating an error table and retrieving rejected errors of the last query via `SELECT rejected_record from TABLE(result_scan(last_query_id()))`
- If we would like to validate entire files, we can set the `RETURN_FAILED_ONLY= = TRUE`. Doing so will only return files which could not be processed. If used with `ON_ERROR = CONTINUE` however, all files will be processed, and only rows containing errors will be skipped
- We can also limit the amount of data loaded during a COPY operation by setting `SIZE_LIMIT` to the maximum number of bytes allowed
- Using `TRUNCATECOLUMNS = TRUE` we can truncate text strings exceeding column length
- Setting `FORCE = TRUE` reloads all data during a COPY operation
- The local load history is available from the `INFORMATION_SCHEMA.LOAD_HISTORY` view, the global history is via `SNOWFLAKE_DB.ACCOUNT_USAGE.LOAD_HISTORY`
- *Unloading* data means exporting data from a table as a file
- Data can be *unloaded* form a table into a stage (BLOB storage) by using the `COPY ` command and switching source and destination, e.g. `COPY INTO @<STAGE_NAME> FROM <TABLE_NAME>`
- Aggregate functions, flatten, group by, filters using WHERE and joins are not supported in the COPY INTO command

### Handling semi-structured data

- Loading raw data requires a stage and a table with a single column of the _variant_ data type
- We can select (JSON) objects and their attributes loaded into a table of _variant_ type using the syntax `SELECT <COLUMN_NAME>:<OBJECT>.<ATTRIBUTE>::<DATA_TYPE> FROM <TABLE_NAME>`
    - We can also use the index of the column instead of the name using `$<INDEX_NUMBER>`
    - you may quote object and attribute with double quotes if it contains spaces
- Use the `flatten` function to flatten nested JSON objects.
- Use `METADATA$<ATTRIBUTE>` to query metadata from a file

### Directory Tables

- Using Directory Tables we can access metadata of staged files via `SELECT * FROM DIRECTORY(@<STAGE_NAME>)`
- Directory Tables need to be explicitly enabled and refreshed before they can be used

### File URLs

- Stages support scoped, file and pre-signed URLs
- Scoped URLs offer temporary access to a file and expire after 24h
- File URLs offer permanent access to a file
- Pre-signed URLs can be used to directly access a file via the browser

### Ingesting data from AWS S3

- Snowflake demands *S3FullAccess* role and trust relationship between AWS Account holding data and Snowflake, to interact with S3
- Interaction is facilitated via storage integration object, but we still need a Stage object for the actual interaction

### Ingesting data from Azure Storage Accounts

- Snowflake demands the *Storage BLOB Data Contributor* role and Azure Consent to interact with Storage Accounts

### Ingesting data from GCP Buckets

- Snowflake demands and the *Cloud Storage Admin* role and a GCP Service Account to interact with GCP Buckets

## Data Transformation

- Snowflake supports SQL:1999 and parts of SQL:2003, e.g. scalar, aggregate, window, table and system functions
- Functions are schema objects

### Estimation Functions

- Use the (HyperLogLog) `HLL` function to estimate number of distinct values 
- Use `APPROX_TOP_K` to estimate the most frequent values
- Use `APPROX_PERCENTILE` to approximate percentiles
- Use `MINHASH` to calculate similarity of two or mor sets

### User Defined Functions (UDFs)

- UDFs support SQL, Python, Java and JavaScript
- We can create UDFs via `CREATE FUNCTION <FUNCTION_NAME>(<PARAMS>) RETURNS <DATA_TYPE> LANGUAGE <LANGUAGE> AS $$ ... $$`

### Stored Procedures (SPs)

- Use SPs to perform database operations.
- SPs support Snowflake Scripting, JavaScript and Python or Java via the Snowpark API
- We can create SPs via `CREATE PROCEDURE <PROCEDURE_NAME>(<PARAMS>) RETURNS <DATA_TYPE> LANGUAGE <LANGUAGE> AS BEGIN ... END`
- SPs can either be run via caller or owner rights

### External Functions (EFs)

- External functions trigger cloud provider functions (e.g. AWS Lambda functions)
- EFs must return scalar values
- EFs can not be shared

### Secure Functions

- Using secured functions, we can hide data and metadata from users, yet doing so will reduces query performance (as optimizer can not be used)

### Sequences

- Think of sequences as Python ranges. It is most often used in CREATE TABLE statements

## Performance Optimization

- As Snowflake architecture is very different from traditional databases, performance optimization centers around choosing *appropriate data types*, *sizing warehouses* correctly (e.g. by scaling up or out) and *caching*
- Results are cached per warehouse and stored for 24 hours or until underlying data changes
- Snowflake can cluster tables (~ add an index) to improve performance, yet is only makes sense for very large tables (multiple TBs)

## Roles and Permissions

- We can create roles via `CREATE ROLE <ROLE_NAME>`
- Roles can be granted privileges using `GRANT USAGE ON <OBJECT> <OBJECT_NAME> TO ROLE <ROLE_NAME>`
- Important global privileges include creating a share, importing and applying masking policies
- Important privileges related to objects include *MODIFY*, *MONITOR*, *OPERATE* (specific to warehouse), *USAGE*, *REFERENCE_USAGE* (for views), *OWNERSHIP* and *ALL*
- Important privileges for stages include *READ*, *USAGE* (for external stage only), *WRITE* (for internal stages only)

## Snowpipe

- Snowpipe monitors buckets for new files and automatically loads and transforms them
- It is build on serverless infrastructure and does not use warehouses (seems like S3 Object Lambda in disguise)
- Snowpipe can also be triggered by REST API endpoints
- Using Snowpipe involves creating an external stage, using the `COPY` command, creating the pipe and setting up a (S3) notification (Queue, EventGridand event subscription with *Storage Queue Data Contributor* role for Azure) to trigger the pipe
- We create pipes using `CREATE OR REPLACE pipe <PIPE_NAME> AS COPY INTO <TABLE_NAME> FROM @<STAGE_NAME>`
- Make sure to set `AUTO_INGEST = TRUE` in order to load files automatically
- Use the `NOTIFICATION_CHANNEL` property of the pipe as the source (SQS Queue) for the S3 event notification
- We can list all pipes using `SHOW PIPES`
- We can update pipe using `ALTER pipe <PIPE_NAME> refresh`
- We can get the status of a pipe using `SELECT SYSTEM$PIPE_STATUS('<PIPE_NAME>')`
- Snowpipe works best with files between 100 and 250 MB

## Time Travel

- Time allows us to access historical data, e.g. data which has been delete or updated. It also allows us to restore dropped schemas, tables databases
- The retention period for time travel depends on the Snowflake editions (default is 1 day, 1 day is also the max for standard, whereas 90 days is the max for other editions)
- We can modify the retention period for a table by editing the `DATA_RETENTION_TIME_IN_DAYS` property. Alternatively, we can set `MIN_DATA_RETENTION_TIME_IN_DAYS` on the account level.
- We can use time travel by either querying data at a specific time stamp via `SELECT * FROM <TABLE_NAME> BEFORE (TIMESTAMP => <TIMESTAMP>)`, or offset via `SELECT * FROM <TABLE_NAME> AT (OFFSET => <NEGATIVE_OFFSET_IN_SECONDS>)`, or before a certain query id via `SELECT * FROM <TABLE_NAME> BEFORE (STATEMENT => <QUERY_ID>)`
- Additionally entire objects can be recovered via `UNDROP <OBJECT> <OBJECT_NAME>`
- Always create a backup table with the results from time travel, then truncate the table we need to fix, and finally insert data from the backup table into the truncated table
- If we need to undrop a table that already exits, e.g. because it has been recreated, try to rename the existing tables before undropping
- Time travel costs results from additional storage. You can get additional information on storage used for different features via `SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.TABLES_STORAGE_METRICS`

## Fail Safe

- Fail safe is the disaster recovery feature in Snowflake
- Data can be recovered for up 7 days after the time travel period has passed (for permanent tables)
- Recovery is done via Snowflake support

## Table Types

- Tables types include permanent, transient and temporary
- Databases can only be permanent or transient
- All table types offer time travel, but only *permanent* tables are **fail safe**
- Transient tables only allow time travel for 1 day
- Temporary tables only exist in the current session
- The type of table is indicated in the *kind* column returned by `SHOW TABLES`

## Zero-Copy Cloning

- Snowflake supports cloning of various objects (databases, schemas, tables, stream, file formats, sequences, tasks - and with some limitations also stages and pipes) via the `CLONE` keyword
- Named Internal Stage and pipes which are not referencing external stages can not be cloned
- If we clone an object, all of its child objects are also cloned
- Cloning is done via zero-copy, meaning only a snapshot of the metadata is duplicated, not the data itself
- Note that, while all child objects will inherit their privileges, the parent object will not.
- In order to clone tables, we need *SELECT* , for pipes, streams and task we need *OWNER*, and for all other objects we need *USAGE* 
privileges
- We can combine cloning and time travel

### Swapping

- We can also use swapping to promote tables and schemas to production.
- Like copying, swapping only affects metadata
- The syntax to swap a table is `ALTER TABLE <TABLE_NAME> SWAP WITH <TABLE_NAME_TO_SWAP_WITH>`

## Replication

- Data can be replicated to another region
- Use `SYSTEM$GLOBAL_ACCOUNT_SET_PARAMETER` to enable replication

## Data Sharing

- Snowflake enables object sharing (tables, external tables, secure views, secure materialized views, secure UDFs) between accounts
- Sharing data does not copy data, instead the producer account grants *read-only* privileges to the consumer account
- Sharing is available for editions except the virtual private edition
- Only a single database can be part of a share
- Only secure materialized views and secure UDFs can be shared
- To create a share, first create it via `CREATE SHARE <SHARE_NAME>`. Then grant privileges for individual objects to the share, i.e. `GRANT USAGE ON DATABASE <DATABASE_NAME> TO SHARE <SHARE_NAME`. Afterwards add consumers to the share via `ALTER SHARE <SHARE_NAME> ADD ACCOUNT <ACCOUNT_ID>`. Finally import the share within the consumer account via `CREATE <OBJECT> <OBJECT_NAME> FROM SHARE <SHARE_NAME>
- Consumers do not necessarily need dedicated Snowflake accounts, as shares can also be made available via *reader* accounts
- Changes of objects within a share object are reflected to consumers immediately. However we might need to explicitly grant permissions to interact with shared objects.
- We can easily grant access to all schemas/tables of a shared database via `GRANT SELECT ON ALL TABLES IN SCHEMA/DATABASE <SCHEMA_NAME/DATABASE_NAME> TO SHARE <SHARE_NAME>`

### Views

- Always use secure views instead of regular views, to avoid sharing sensitive (meta-) data (for example definition data) by accident
- Secure views are created via `CREATE OR REPLACE SECURE VIEW <VIEW_NAME> AS SELECT [...]`
- Secure view can reference data from several databases, if all database have granted reference usage on the share via `GRANT REFERENCE_USAGE ON DATABASE <DATABASE_NAME> TO SHARE <SHARE_NAME>`

## Data Sampling

- Sampling data is useful to enable cost-efficient development on very large tables
- Sampling can be done via the *row* (Bernoulli) or *block* (System) method.
- The *row* method samples every with a given percentage, while the method samples every block (micro-partition)
- The block method is recommended for very large tables

## Tasks

- Use tasks to schedule SQL statements or stored procedures
- Tasks can be created via `CREATE OR REPLACE TASK <TASK_NAME> SCHEDULE = '<SCHEDULE_STRING>' AS <SQL_QUERY>` - where <SCHEDULE_STRING> can be an interval or a CRON job, i.e. `USING CRON minute hour day_of_month month day_of_week time_zone` 
- Tasks can be assigned warehouses for execution explicitly or use severless compute
- Tasks can be suspended or resumed via `ALTER TASK <TASK_NAME> SUSPEND/RESUME`
- Tasks can be executed in order by creating tree of tasks
- Use `SELECT SYSTEM$TASK_DEPENDENTS_ENABLE('<TASK_NAME>')` to recursively resume task trees
- Tasks obey conditions using the *WHEN* keyword. Conditions do not support functions
- We can access task history via `SELECT * FROM TABLE(information_schema.task_history()) ORDER BY scheduled_time desc`

## Streams

- Stream objects are used to conduct change-data-capture (CDC)
- Stream objects include the changed data and the metadata columns: action, update and row_id
- Under the hood streams use offsets to capture snapshots of tables at a given point in time
- Streams become stale after a maximum of 14 days and can't be consumed any more
- Once a stream has been *consumed* (via INSERT, UPDATE, or DELETE), it is empty
- We can create streams via `CREATE OR REPLACE STREAM <STREAM_NAME> on TABLE <TABLE_NAME>`
- Streams do not automatically handle CDC operations for us, meaning we need to write logic to insert, update or delete changes captured in streams
- Streams can be combined with tasks to automatically execute logic via `CREATE OR REPLACE TASK <TASK_NAME> WHEN SYSTEM$STREAM_HAS_DATA('<STREAM_NAME>') AS ...`
- Streams can be configured as *append-only* by setting `APPEND_ONLY=TRUE` (which may improve performance a lot)
- We can also use `CHANGES` together with `OFFSET` in order to have more control over the offset

## Materialized Views

- Materialized Views combine the benefits of a view (data from source tables are always up to date) and a table (data is persisted, improving performance)
- Materialized Views are a good option to abstract complex, long-running query logic which is used often, yet the underlying data is changing infrequently
- Materialized Views are only available from the Enterprise Edition onwards and do not support joins, UDFs, and some aggregate functions
- Maintenance (refreshing) of materialized views is managed by Snowflake and incurs some cost
- Details about costs can be investigated via `SELECT * FROM TABLE(INFORMATION.SCHEMA.MATERIALIZED_VIEW_REFRESH_HISTORY)`
- We can create materialized views via `CREATE OR REPLACE MATERIALIZED VIEW <VIEW_NAME> AS SELECT [...]`

## Dynamic Data Masking (DDM)

- DDM is a form of column-level security which allows different roles to see different kinds of data
- We need to create a masking policy to use DDM via `CREATE OR REPLACE MASKING POLICY <POLICE_NAME> AS [...]` and apply it to a specific column via `ALTER TABLE <TABLE_NAME> MODIFY COLUMN <COLUMN_NAME> SET MASKING POLICY <POLICY_NAME>`
- Once a policy is applied to a column, it can not be deleted. Therefore, use `SELECT * FROM TABLE(INFORMATION_SCHEMA.POLICY_REFERENCES(policy_name => <POLICY_NAME>))` to get information about applied policies and unset them
- We can update masking policies via `ALTER MASKING POLICY <POLICY_NAME> SET BODY -> [...]`

## Search Optimization (SO)

- SO adds a search access path to queries
- SO can improve the performance of lookup and analytical queries
- SO is a serverless feature
- SO can be activated via `ALTER TABLE <TABLE_NAME> ADD SEARCH OPTIMIZATION (ON ...)`

## Security

- Data in Snowflake is encrypted in transit and on rest
- Key rotation happens every 30 days
- Re-keying can also be enabled with the Enterprise Edition. Doing so will change keys every year
- Tri-Secret Secure is a feature from the Business Critical Edition, which enables customers to use customer-managed keys + Snowflake-managed keys
- Secure the admin account with MFA
- Use MFA or key pair authentication to secure user accounts
- Use row access policies to apply row-level security
- Policies are created via `CREATE OR REPLACE ROW ACCESS POLICY <POLICYNAME> ...`
- Policies can be applied to policies via `ALTER TABLE <TABLE_NAME> ADD ROW ACCESS POLICY <POLICY_NAME>`

## Performance

- Caching in Snowflake happens in 3 areas:
    - Result and metadata cache in cloud services
    - Data Cache in compute layer
    - Remote Disk (Cache) in storage layer
- The Data Cache is a local SSD cache tied to a virtual warehouse and can not be shared

## Best Practices

- Enable auto-suspend and auto-resume for warehouses per default
- Adjust timeouts for auto-suspend based on workloads
- Choose warehouses sizes based on workloads
- Use transient tables for development and staging
- Use cluster keys if absolutely necessary
- Disable time travel for transient tables, e.g. for staging
- Use 4-7 days time travel for production tables
- Potentially disable time travel for very large, high-churn tables (rely on fail-safe instead)

## Misc

- Releases are deployed weekly
- Use the *ACCOUNT_USAGE* schema to query historical usage data. It also includes information about dropped objects. Alternatively we can use the *READER_ACCOUNT_USAGE* schema which includes more limited information
- The *INFORMATION_SCHEMA* can also be used to query historical usage data, but it has shorter retention time (7 days - 6 months), yet no latency (in comparison to *ACCOUNT_USAGE* schema)
- 


[1]: https://www.udemy.com/course/snowflake-masterclass
[2]: https://www.udemy.com/course/snowflake-certification-snowpro-core-exam-prep
[3]: https://docs.snowflake.com/en/user-guide/snowsql
