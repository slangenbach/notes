# AWS ML Specialty 

## Kinesis

* Data streams: Think real-time
* Firehose: Think ingestion
* Analytics: Think ETL
* Video streams: Think video ;-)

### Analytics

* is built on Apache Flink
* Input can come from Kinesis Data Streams or Firehose
* We can use lookup tables (stored in S3) while analyzing the stream
* Lambda functions can be used for stream processing
* Kinesis comes with built in anomaly detection (Random Cut Forrest [uses only recent history]), and detection of dense locations (HOTSPOTS) - both are implemented as SQL functions

### Video Streams

* meant to stream data from cameras
* can playback video
* Video content can also be send to AI services, e.g. Recognition

## Glue

### Data catalog