# aws-certified-database-specialty-exam-notes
Notes for the AWS Certified Database – Specialty

# Amazon RDS

* 3 Storage Types available to RDS
  * General Purpose SSD (gp2)
    * Broad use cases.
    * Low latencies and burst to 3K IOPS.
    * Deliver baseline and burst performance 99% of the time
  * Provisioned IOPD SSD (io1)
    * IO Intensive workloads.
    * Good for heavy loads, consistency throughput, low latency.
    * Deliver their provisioned performance 99.9% of the time.
  * Magnetic Storage
    * Legacy.
    * Only used for very special needs.

# Amazon Aurora

* Compatible with MySQL and POstgreSQL
* [What is Amazon Aurora?](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html)
* Endpoints
  * Cluster (or Writer) Endpoint (cannot be user modified)
  * Reader Endpoint (cannot be user modified)
  * Instance Endpoint (specific db instance)
  * Custom Endpoint
* Replica Priorities
  * 0 (first) to 15 (last)
* Aurora Database Activity Stream
  * synchronous mode (higher impact, not available for Aurora MySQL)
  * asynchronous mode (lower impact)
  * [Database Activity Streams](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/DBActivityStreams.html)
  * Amazon Kinesis is required for database activity streams.

# Amazon Aurora Serverless

* [Restoring an Aurora Serverless v1 DB Cluster](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.restorefromsnapshot.html)
  * *Minimum Aurora capacity unit* – Aurora Serverless can reduce capacity down to this capacity unit.
  * *Maximum Aurora capacity unit* – Aurora Serverless can increase capacity up to this capacity unit.
  * *Timeout action* - The action to take when a capacity modification times out because it can’t find a scaling point.
  * *Pause after inactivity* -  The amount of time with no database traffic to scale to zero processing capacity. When database traffic resumes, Aurora automatically resumes processing capacity and scales to handle the traffic.



# Amazon DynamoDB

* A local secondary index maintains an alternate sort key for a given partition key value. A local secondary index also contains a copy of some or all of the attributes from its base table; you specify which attributes are projected into the local secondary index when you create the table. The data in a local secondary index is organized by the same partition key as the base table, but with a different sort key. This lets you access data items efficiently across this different dimension. For greater query or scan flexibility, you can create up to five local secondary indexes per table. [Local Secondary Indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/LSI.html)
* A DynamoDB TTL Index process can take up to 48 hours to delete an expired item.
* [TTL Index](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html)

# Amazon DAX

# Amazon Redshift

* Performance Tuning
  * Automatic workload management (WLM)
    * Use machine learning to dynamically manage memory and concurrency helping maximize query throughput
  * Set priority of queries.
  * Short query acceleration (SQA)
    * prioritizes selected short-running queries ahead of longer-running queries. 
    * SQA runs short-running queries in a dedicated space.
    * No waiting behind long-running queries.
  * Concurrency Scaling
    * Scale cluster as needed (£)

# Amazon ElastiCache

* Elasticache for Redis
  * If you require data durability, you can enable the Redis append-only file feature (AOF). When this feature is enabled, the node writes all of the commands that change cache data to an append-only file. When a node is rebooted and the cache engine starts, the AOF is "replayed." The result is a warm Redis cache with all of the data intact. [Append only files (AOF) in ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/RedisAOF.html)

# Amazon DocumentDB

* Amazon DocumentDB (with MongoDB compatibility) keeps a record of events that relate to your clusters, instances, snapshots, security groups, and cluster parameter groups. This information includes the date and time of the event, the source name and source type of the event, and a message that is associated with the event.[DocumentDB Events](https://docs.aws.amazon.com/documentdb/latest/developerguide/managing-events.html)
  * Audit Logs are published to [CloudWatch Logs](https://docs.aws.amazon.com/documentdb/latest/developerguide/event-auditing.html)

# Amazon Neptune

# Amazon Database Migration Service

* AWS Data Migration Service (AWS DMS) helps you migrate your data to and from widely used commercial and open-source databases, including Oracle, MySQL, and PostgreSQL. The service supports homogeneous migrations such as Oracle to Oracle, and heterogeneous migrations such as Oracle to PostgreSQL or MySQL to Oracle.

* Databases can be migrated with severall tasks, i.e. one for each table.
* Tasks can work concurrently and idependently.
* AWS DMS loads eight tables at a time by default.
* Large tables can be broken up into multiple tasks.

# Amazon Schema Migration Tool

* AWS Schema Conversion Tool (AWS SCT) makes heterogeneous database migrations easy by automatically converting the source database schema and a majority of the custom code to a format compatible with the target database.