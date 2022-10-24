# AWS Data Specialty

## General

## Collection

### Kinesis

* If you read Kinesis Data Streams, think Kafka topics
* Producers can be apps (SDK), clients (written using Kinesis Producer Library, KPL) Kinesis agents
* Consumers can be apps (SDK), clients (written using Kinesis Consumer Library, KCL, Lambda functions, Kinesis Data Firehose or Kinesis Data Analytics
* Records sent to data streams contain _partition key_ and _data blob_
* Partition key determines to which _shard_ data is written
* Retention can be between 1 and 365 days
* Kinesis Data Streams can be created in _provisioned_ (define shards upfront and pay per shard per hour) or _demand_ (automatic scaling - based on throughput of last 30 days - payed per stream per hour & data in/output per GB) mode
* If you capacity in advance, go for _provisioned mode_
* Kinesis Data Streams support VPC endpoints

#### Producers

##### SDK

* Use SDK for low throughput, high latency
* SDK exposes PutRecord (single record) and PutRecord*s* (batched records) method
* ProvisionedThroughPutExceed exception is thrown when data/records per second exceeds threshold of shard
* Choose your partition key wisely

##### KPL

* Java/C++ library
* Use KPL to build high throughput, long-running producers
* Supports _sync_ and _async_ API -> If you read sending data to Kinesis asynchronously, think KPL
* Submits metrics to Cloudwatch
* Supports batch via _collect_ (write to multiple shards in one PutRecords API call) and _aggregate_ (nest multiple records in a single record)
* Compression is *not* supported out of the box
* KPL created records can only be read with the KCL library
* Don't use KPL if latency is important or if only latest events are of interest

##### Agent

* built on top of KPL
* watches files/directories and can send to multipe streams
* can also preprocess and convert data before sending it
* Supports file rotation, checkpointing and retries