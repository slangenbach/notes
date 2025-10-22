# DBT Zero to Hero

Notes on the [course][1] from Udemy.

## General

- DBT focuses on transforming raw data already loaded into the data warehouse (DWH)
- It brings software engineering best practices to data transformation

## Theory

- ETL performs transformation before writing data to the DWH, while ELT directly loads data into the DWH and transforms it in there (pattern used by Snowflake, AWS Redshift, Google BigQuery etc.)
- ELT pipelines are more flexible, scale better and are easier to test and debug than ETL pipelines. Yet they do consume more storage than ETL pipelines as raw data is directly written into the DWH
- A lakehouse combines the scalability and low-cost storage of a data lake with the management, ACID transactions, and performance features of a DWH
- Traditional DWHs rely on symmetric multiprocessing (SMP) where compute and storage are tightly coupled. Thus they can only scale vertically. Modern DWHs use massively parallel processing (MPP), where compute and storage are decoupled, and thus horizontal scaling is possible (c.f. [Designing Data-Intensive Applications][2])
- Slowly changing dimensions (SCD) are dimension attributes whose values change slowly over time.
- SCD Type 0 does **not** update the DWH table when a dimension changes (retain original)
- SCD Type 1 updates the DWH table if a dimensions changes by simply overwriting it (overwrite)
- SCD Type 2 adds a new row to the DWH table if a dimensions changes, thereby keeping the full history of previous values (add new row)
- SCD Type 3 is a variant of type 2, where only the previous value is kept. If a dimensions changes, the previous value is written to a separate column in the same DWH table and the actual value is updated. Doing so reduces the amount of data (rows) used to store history (add new column)


[1]: https://www.udemy.com/course/complete-dbt-data-build-tool-bootcamp-zero-to-hero-learn-dbt
[2]: https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/
