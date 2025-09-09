# Snowflake SnowPro Core

Notes on the [course][1] from Udemy.

## General


## Architecture

- Snowflake separates storage, compute (called _warehouse_ in Snowflake lingo) and additional services
- Warehouses sizes range from XS (1 server) to 6-XL (128 servers)
- Data is stored in BLOB storage as hybrid columnar storage
- Warehouses can be scaled horizontally using _standard_ and _economy_ policies. _Standard_ policy focuses on minimize queuing of queries (starts new cluster if when its needed), while _economy_ focuses on preserving credits (starts new cluster only if loaded for approx. 6min)


[1]: https://www.udemy.com/course/snowflake-masterclass
