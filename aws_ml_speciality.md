# AWS ML Specialty 

## General

* Know about the curse of dimensionality (exploding solution space)
* Know how to handle missing data. i.e. dropping, replacement with mean, imputing with ML techniques (Regression [MICE], KNN, etc.), getting more data
* Know how to handle imbalanced data, i.e. oversampling (duplicate minority classes [SMOTE]), undersampling, 
* Know how to handle outliers, i.e. removing (filter by IQR or std. dev, using Random Cut Forest algorithm, etc.)
* Know what binning (putting observations in ranges of values), encoding/scaling are
* Know about confusion matrices, precision, recall/sensitivity (true positive rate, useful for fraud detection), F1 and ROC, AUC and their formulas
* Know about bagging (multiple models) and boosting (sequence models)

## Data Engineering

### Kinesis

* If you read real-time, think Kinesis
* There exist 4 Kinesis services: Data streams, Analytics, Firehose and Video Streams

#### Data Streams

* Streams are divided into shards/partitions
* Streams allow custom code for producer/consumer and is real-time
* Shards need to be provisioned in advance
* 24h default retention
* Maximum record size is 1MB
* Producer: 1 MB/s or 1000 messages/shard
* Consumer: 2 MB/s per shard

#### Analytics

* If you read Kinesis Analytics, think ETL, i.e. real-time analysis on streams using SQL
* is built on Apache Flink
* Input can come from Kinesis Data Streams or Firehose
* We can use lookup tables (stored in S3) while analyzing the stream
* Lambda functions can be used for stream processing
* Kinesis comes with built in anomaly detection (Random Cut Forrest [uses only recent history]), and detection of dense locations (HOTSPOTS) - both are implemented as SQL functions

#### Firehose

* If you read Kinesis Firehose, think ingestion, i.e. load streams into S3/RedShift/ElasticSearch/3rd-party-tools (Splunk)/HTTP endpoints as batches (near-real-time)
* allows converting records to Parquet or ORC
* also supports compression and encryption

#### Video Streams

* If you read Kinesis Video Streams, think video, i.e. stream video in real-time ;-)
* meant to stream data from cameras
* can playback video
* Video content can also be send to AI services, e.g. Recognition

### Glue

#### Data Catalog

* Think metadata repository
* Crawlers work on JSON, Parquet, CSV, relational databases and on services like S3, RDS, Redshift
* Crawlers automatically infer schemas and create tables from S3 buckets

#### ETL

* Supports Python/Scala and PySpark
* Supports bundled transformations, i.e. DropFields, Filter, etc. and machine learning transformations, i.e. FindMatches (fuzzy matching)

### Athena

* tbd

### Data Stores

* Redshift: Warehousing, Columnar storage
* RDS/Aurora: Relational database
* Dynamo DB: NoSQL database
* S3: Object storage
* ElasticSearch: Clickstream analytics and search
* ElastiCache: Caching

### Data Pipelines

* Orchestrator to transfer data from AWS/on-premise to another AWS service
* In comparison to Glue you have more control over resources and configuration

### Batch

* Serverless service
* Batch jobs are based on Docker images
* Supports any type of workload, but focuses on non-ETL workloads

### Database migration service (DMS)

* Support homo- (Oracle to Oracle) and heterogenous (Oracle to SQL Server) migrations
* Continuous service

### Step functions

* If you read workflow, think step functions
* Can be used to manage batch jobs, e.g. using Glue

## Exploratory data analysis

* Types of data (continuous, discrete, categorical, ordinal)
* Data distributions, probability density function (continuos) / probability mass function (discrete data)
* Poisson distribution, binominal distribution, Bernoulli distribution (single trial)
* Time series = seasonality + trends + noise

### AWS Tooling for EDA

* Athena
* QuickSight (SPICE engine for in-memory calculations, Insights for anomaly detection and forecasting)
* EMR (managed Hadoop using EC2 instances, can use S3 as file system using EMRFS)

### AWS Tooling for data labeling

* SageMaker Ground Truth (plus)
* AWS Rekognition (image recognition)
* Comprehend (text analysis)

## Modeling

* Activation functions (sigmoid/logistic [0-1], tanh [-1-1], ReLu [0 or positive], leaky ReLu [can have negative values], softmax, etc.)
* Use softmax for multiple classification, tanh for RNN, and start with ReLu for everything else
* Use CNN for image analysis, RNNs for time-series data or data that consists for sequences of arbitrary length
* RNN topologies?
* Regularization techniques for neural networks include dropout (drop out neurons at random) and early stopping
* L1 regularization (remove features, inefficient, sparse output), L2 regularization (adjust weight of features, efficient, dense output)
* Vanishing gradient problem: Slope of learning curve approaches zero, things get stuck as we work with small numbers that slow down training or introduce errors - Use LSTM nets or ResNet to deal with it
* Exploding gradient problem: 