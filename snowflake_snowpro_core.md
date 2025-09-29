# Snowflake SnowPro Core

Notes on the [course][1] from Udemy.

## General

- Snowflake offers Standard, Enterprise,  Business Critical and Virtual Private editions
- Enterprise offers multi-cluster warehouses, 90 day time travel, and other features
- Business Critical
- Virtual Private edition is made up of a dedicated services and needs to be setup on a by case basis

## Architecture

- Snowflake separates storage, compute (called _warehouse_ in Snowflake lingo) and additional services
- Warehouses sizes range from XS (1 server) to 6-XL (128 servers)
- Data is stored in BLOB storage as hybrid columnar storage
- Warehouses can be scaled horizontally using _standard_ and _economy_ policies. _Standard_ policy focuses on minimize queuing of queries (starts new cluster if when its needed), while _economy_ focuses on preserving credits (starts new cluster only if loaded for approx. 6min)

## Pricing

- Snowflake charges for compute, storage and data transfer. Compute and storage can be scaled individually. Charging is done via credits and varies by cloud provider and region
- Compute charges for active warehouses (billed by second, charged per hour), cloud services and other internal services
- Storage charges for storage used in TB (billed monthly, charged by average storage used per month). Users can save on storage costs by choosing capacity-based charging over on-demand based charging.
- Data ingestion is free, egress incurs is charged for. Costs vary by cloud provider and region. Inter-region and inter-cloud-provider transfer is especially costly.
- Costs can be monitored via resource monitors

## Identity and Access Management

- Org Admin > Account Admin > Security Admin /  Sys Admin > User Admin (Sec) / Custom Roles (Sys Admin) > Public (User Admin)

## Data Ingestion

- Data can be ingested in bulk or continuously
- A stage in Snowflake refers to an object where data metadata is stored, e.g. its location and the files it contains
- External stages refer to object storage of a cloud provider, e.g. AWS S3 bucket
- Internal stages refer to local file storage
- We refer to external stages using @STAGE_NAME

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
- Data can be *unloaded* form a table into a stage (BLOB storage) by using the `COPY ` command and switching source and destination, e.g. `COPY INTO @<STAGE_NAME> FROM <TABLE_NAME>`

### Handling semi-structured data

- Loading raw data requires a stage and a table with a single column of the _variant_ data type
- We can select (JSON) objects and their attributes loaded into a table of _variant_ type using the syntax `SELECT <COLUMN_NAME>:<OBJECT>.<ATTRIBUTE>::<DATA_TYPE> FROM <TABLE_NAME>`
    - We can also use the index of the column instead of the name using `$<INDEX_NUMBER>`
    - you may quote object and attribute with double quotes if it contains spaces
- Use the `flatten` function to flatten nested JSON objects.
- Use `METADATA$<ATTRIBUTE>` to query metadata from a file

### Ingesting data from AWS S3

- Snowflake demands *S3FullAccess* role and trust relationship between AWS Account holding data and Snowflake, to interact with S3
- Interaction is facilitated via storage integration object, but we still need a Stage object for the actual interaction

### Ingesting data from Azure Storage Accounts

- Snowflake demands the *Storage BLOB Data Contributor* role and Azure Consent to interact with Storage Accounts

### Ingesting data from GCP Buckets

- Snowflake demands and the *Cloud Storage Admin* role and a GCP Service Account to interact with GCP Buckets

## Performance Optimization

- As Snowflake architecture is very different from traditional databases, performance optimization centers around choosing *appropriate data types*, *sizing warehouses* correctly (e.g. by scaling up or out) and *caching*
- Results are cached per warehouse and stored for 24 hours or until underlying data changes
- Snowflake can cluster tables (~ add an index) to improve performance, yet is only makes sense for very large tables (multiple TBs)

## Roles and Permissions

- We can create roles via `CREATE ROLE <ROLE_NAME>`
- Roles can be granted privileges using `GRANT USAGE ON <OBJECT> <OBJECT_NAME> TO ROLE <ROLE_NAME>`

## Snowpipe

- Snowpipe monitors buckets for new files and automatically loads and transforms them
- It is build on serverless infrastructure and does not use warehouses (seems like S3 Object Lambda in disguise)
- Using Snowpipe involves creating an external stage, using the `COPY` command, creating the pipe and setting up a (S3) notification (Queue, EventGridand event subscription with *Storage Queue Data Contributor* role for Azure) to trigger the pipe
- We create pipes using `CREATE OR REPLACE pipe <PIPE_NAME> AS COPY INTO <TABLE_NAME> FROM @<STAGE_NAME>`
- Make sure to set `AUTO_INGEST = TRUE` in order to load files automatically
- Use the `NOTIFICATION_CHANNEL` property of the pipe as the source (SQS Queue) for the S3 event notification
- We can list all pipes using `SHOW PIPES`
- We can update pipe using `ALTER pipe <PIPE_NAME> refresh`
- We can get the status of a pipe using `SELECT SYSTEM$PIPE_STATUS('<PIPE_NAME>')`

## Time Travel

- Time allows us to access historical data, e.g. data which has been delete or updated. It also allows us to restore dropped schemas, tables databases
- The retention period for time travel depends on the Snowflake editions (default is 1 day, 1 day is also the max for standard, whereas 90 days is the max for other editions)
- We can modify the retention period for a table by editing the `DATA_RETENTION_TIME_IN_DAYS` property

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
- If we clone an object, all of its child objects are also cloned
- Cloning is done via zero-copy, meaning only a snapshot of the metadata is duplicated, not the data itself
- Note that, while all child objects will inherit their privileges, the parent object will not
- In order to clone tables, we need *SELECT* , for pipes, streams and task we need *OWNER*, and for all other objects we need *USAGE* 
privileges
- We can combine cloning and time travel

### Swapping

- We can also use swapping to promote tables and schemas to production.
- Like copying, swapping only affects metadata
- The syntax to swap a table is `ALTER TABLE <TABLE_NAME> SWAP WITH <TABLE_NAME_TO_SWAP_WITH>`

## Data Sharing

- Snowflake enables object sharing (tables, external tables, secure views, secure materialized views, secure UDFs) between accounts
- Sharing data does not copy data, instead the producer account grants *read-only* privileges to the consumer account
- Sharing is available for editions except the virtual private edition
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


[1]: https://www.udemy.com/course/snowflake-masterclass
