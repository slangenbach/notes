# Snowflake SnowPro Core

Notes on the [course][1] from Udemy.

## General


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
- Data can be transformed during loading into stages by using SQL syntax (and certain SQL functions) within the `FROM` clause of the `COPY INTO` command
- We can also only load data into certain columns when copying data into a stage
- We can handle errors during data ingestion using the `ON_ERROR` keyword. Options include _ABORT_STATEMENT_, _SKIP_FILE_ (we can also specify the threshold of errors at which we skip a file using SKIP_FILE_<THRESHOLD>) and _CONTINUE_ 
- Stages and file formats can be saved into dedicated objects within dedicated schemas within a dedicatd databases (e.g. MANAGE_DB)

[1]: https://www.udemy.com/course/snowflake-masterclass
