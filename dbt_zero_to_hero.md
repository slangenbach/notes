# dbt Zero to Hero

Notes on the [course][1] from Udemy.

## General

- dbt focuses on transforming raw data already loaded into the data warehouse (DWH)
- It brings software engineering best practices to data transformation
- dbt can be extended via 3rd packages from [dbt hub][5]

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
- Use [Common Table Expressions (CTE)][3] to deal with complex SQL statements. They are ephemeral constructs which only exist during query execution

## Models & Materialization

- Models are the basic building block of business logic
- They are defined as SQL files
- Models can be materialized as views, tables, incremental (by appending to tables) or ephemeral (as CTEs)
- Use `dbt run --full-refresh` to rebuild all tables
- Check out the _target_ folder to inspect the SQL compiled by dbt
- Use `dbt compile` to test your dbt code

## Seeds & Sources

- Seeds are local files which are uploaded to the DWH via dbt
- Sources are abstraction layers on top of input tables. You can use them to reference input tables in your SQL statements without hardcoding them
- Sources can be checked for freshness by implementing checks for a timestamp in the source table

## Snapshots

- Snapshots keep track of changes to your data using SCD type 2
- dbt can use the _timestamp_ (use unique key and updated_at field to determine changes) and _check_ (any change in a set of all columns will be picked up as an update) strategies for snapshotting
- You can also implement custom snapshotting logic via macros

## Tests

- dbt support units and data tests
- Unit tests test transformations with mock data
- Data tests (~integration tests) test data integrity and quality against real data
- Unit tests can be specified using a _yaml_ file including name, model, given and expected fields
- Data tests supports singular (SQL queries expected to return an empty result set) and generic (unique, not_null, accepted_values, relationships)
- We can define custom generic tests and use tests provided by 3rd party packages
- Tests can have different severities (_warn_ or _error_)
- Contracts enforce the schema of models
- Use `dbt test -x` to stop after the first failing test. Use `dbt test -s <TEST_NAME>` to target a specific taks
- Add `+store_failures: true` to `dbt_project.yml` to save test failures to the DWH

## Macros

- dbt makes heavy use of [Jinja][4] and adds custom Jinja features such as `ref` and `source`
- Macros are reusable blocks of Jinja code (similar to function in programming languages)
- Macros start with `{% macro MACRO_NAME(PARAM_NAME) %}` and end with `{% endmacro %}`
- Adding a dash `-` before and/or after Jina tags will strip whitespace

## Documentation

- Documentation can be inserted into YAML files (using _description_ keyword with strings or Jinja placeholders), and/or in Markdown files
- We can also customize the overview page by writing custom Markdown inside a docs Jinja block
- Use `dbt docs generate` to compile documentation and `dbt docs serve` to view it

## Logging

- We can log custom message by using the {{ log("LOG_MESSAGE", info=<TRUE|False>) }} macro
- Setting `info=True` logs messages to stdout and to the _dbt.log_ file

## Variables

- Variabels can be defined in _dbt_project.yml_
- Use `var("VAR_NAME")` to access variables

## Analyses

- Analyses enables us to execute ad-hoc queries using dbt macros

## Hooks

- Hooks allow us to execute SQL queries on certain events
- Hook types include _on-run-start_, _on-run-end_, _pre-hook_ and _post-hook_

## Grants

- Grants enable us to grant permissions to roles within dbt using YAML syntax

## Fusion Engine

- dbt Fusion is a re-implementation of the dbt core logic in Rust. It also has built-in SQL compiler

[1]: https://www.udemy.com/course/complete-dbt-data-build-tool-bootcamp-zero-to-hero-learn-dbt
[2]: https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/
[3]: https://airbyte.com/data-engineering-resources/common-table-expression
[4]: https://en.wikipedia.org/wiki/Jinja_(template_engine)
[5]: https://hub.getdbt.com/
