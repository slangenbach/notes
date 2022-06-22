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
* Exploding gradient problem: tbd

### SageMaker
* Supports automatic hyperparameter optimization, which Finds the best parameters given a range of values
* Integrates with Spark via sagemaker-spark library
* Debugger allows to inspect models during training stage
* Autopilot does AutoML and integrates with SageMaker Clarify
* Model Monitor monitors data/model quality
* Supports templates/pre-trained models via JumpStart

#### Algorithms

##### Linear Learner
* Linear learner for regression and classification 
* Accepts RecordIO and CSV inputs
* Can do normalization of inputs automatically
* Can use different optimization algorithms, e.g. SGD
* Can train multiple models in parallel

##### XGBoost
* Can be used for regression and classification
* Accepts CSV, libsvm, RecordIO and parquet as input
* Performance is memory-bound
* Can be trained on GPUs
* Hyperparameters one might see on the exam:
    - subsample
    - eta (step size shrinking)
    - gamma (minimum loss reduction to create partition)
    - alpha (L1 regularization)
    - lambda (L2 regularization)
    - eval_metric
    - scale_post_weight (handle unbalanced data by adjusting balance of positive/negative weights)
    - max_depth (of tree)

##### Seq2Seq
* Can be used for machine translation, text summaries, speech-to-text, etc.
* Accepts RecordIO (integers)
* Pre-trained models and dedicated training sets are available
* Hyperparameters one might see on the exam:
    - batch_size
    - optimizer_type
    - learning_rate
    - num_layers_encoder
    - num_layers_decoder
* Can optimize on accuracy, BLEU score (comparison against multiple reference translations), perplexity
* Uses GPU and can use multiple GPUs on a single instance

##### DeepAR
* Can forecast one-dimensional time series
* Automatically finds frequencies and seasonality
* Accepts JSON and parquet
* Hyperparameters one might see on the exam:
    - context_length (number of time points model sees before making prediction)
    - epochs
    - mini_batch_size
    - learning_rate
    - num_cells
* Can use CPU and GPU instances

##### BlazingText
* Can be used for text classification (only sentences, not entire documents)
* Also supports Word2Vec
* Accepts text as input (one sentence per line)

##### Object2Vec
* Like Word2Vec but generalized to objects, i.e. similar products
* Accepts paired attributes (tokenized inputs)
* Inputs -> Encoders -> Comparator -> Label
* Can only train on single instance

##### Object Detection
* Identifies objects in images :-)
* Can train from scratch or use pre-trained models
* Accepts RecordIO or images (including JSON annotation file)

##### Image Classification
* Classifies images
* Accepts RecordIO (MXNet format) or images (including annotation file)

##### Semantic Segmentation
* Can be used for pixel-level object classification
* Accepts images (including annotation file)
* Build on MXNet Gluon/Gluon CV

##### Random Cut Forest
* Can be used for anomaly detection
* Unsupervised algorithm
* Accepts RecordIO and CSV input
* Identifies anomalies by looking at expected change in complexity of tree as result of adding a data point to it

##### Neural Topic Model
* Can be used to organize documents into topics
* Unsupervised algorithm
* Accepts RecordIO and CSV
* Words must be tokenized first

##### LDA
* Another topic modeling algorithm (yet not based on deep learning)
* Hyperparameters one might see on the exam:
    - num_topics
    -alpha0
* Can only train on a single CPU instance

##### KNN
* Can be used for simple classification (find most frequent value) and regression (find average value) problems
* Accepts RecordIO and CSV
* Version built into SageMaker includes data sampling and dimensionality reduction

##### K-Means
* Can be used for unsupervised clustering
* Version built into SageMaker scales very well

##### PCA
* Can be used for dimensionality reduction

##### Factorization machines
* Can be used with sparse data and thus is useful for click predictions, recommendations, etc.
* Supports regression and classification

##### IP Insights
* Can be used to identify suspicious behavior from IP addresses
* Only accepts CSV input

##### Reinforcement Learning
* Q-Learning
* Is based on Tensorflow and MXNet
* Version built into SageMaker supports distributed training

#### Hyperparameter tuning
