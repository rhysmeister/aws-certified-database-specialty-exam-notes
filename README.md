# aws-certified-database-specialty-exam-notes
Notes for the AWS Certified Database – Specialty

# Amazon RDS

| Type | Max Replicas | Serverless Option | DB Cloning | Blue/Green Deployments |
| --- | --- | --- | --- | --- |
| MySQL | 15 | No | No | Yes |
| MariaDB | 15 | No | No | Yes |
| PostgreSQL | 15 | No | No | No |
| Oracle | 5 | No | No | No |
| MSSQL | 5 | No | No | No |
| Aurora MySQL | 15 | Yes | Yes | Yes |
| Aurora PostgreSQL | 15 | Yes | Yes | No |

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

# RDS Trivia

* It is possible to replicate from a on-premises MySQL server to RDS using binary logging.
* Amazon RDS Performance Maintenance on a Multi-AZ Deployment by...
  * Perform maintenance on the standby
  * Promote the standby to primary
  * Performance maintenance on the old primary, which is now the standby
  * N.B. Only OS Update - When the DB Engine is updated both the primary & secondary instances are shutdown at the same time.
* [Setting up Windows Authentication for SQL Server DB instances](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_SQLServerWinAuth.html#USER_SQLServerWinAuth.SettingUp)
  * An AWS Managed Microsft AD is used.
  * Can add trusts to on-prem AD (forst trust)

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
* Backup & Encryption Notes
  * Aurora Clusters cannot be modified to disable encryption once encrypted.
  * Cannot convert an unencrypted cluster to an encrypted one.
  * However, you can restore an unencrypted backup to an encrypted Aurora Cluster.
    * Specify a KMS encryption key during restore.
  * You cannot convert an Aurora snapshot to an encrypted snapshot
    * This is however possible in Amazon RDS.

# Amazon Aurora Serverless

* [Restoring an Aurora Serverless v1 DB Cluster](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.restorefromsnapshot.html)
  * *Minimum Aurora capacity unit* – Aurora Serverless can reduce capacity down to this capacity unit.
  * *Maximum Aurora capacity unit* – Aurora Serverless can increase capacity up to this capacity unit.
  * *Timeout action* - The action to take when a capacity modification times out because it can’t find a scaling point.
  * *Pause after inactivity* -  The amount of time with no database traffic to scale to zero processing capacity. When database traffic resumes, Aurora automatically resumes processing capacity and scales to handle the traffic.

# Amazon DynamoDB

* Non-relational, key-value/wide-column and document database.
* Single-digit millisecond performance.
* Fully managed database with built-in security, backup and restore, and in-memory caching.
* Internet-scale application.
* Max Item Size = 400KB
  * The attribute name counts towards the size limit.
* A local secondary index maintains an alternate sort key for a given partition key value. A local secondary index also contains a copy of some or all of the attributes from its base table; you specify which attributes are projected into the local secondary index when you create the table. The data in a local secondary index is organized by the same partition key as the base table, but with a different sort key. This lets you access data items efficiently across this different dimension. For greater query or scan flexibility, you can create up to five local secondary indexes per table. [Local Secondary Indexes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/LSI.html)
* A DynamoDB TTL Index process can take up to 48 hours to delete an expired item.
* [TTL Index](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/TTL.html)
* [Local & Global Secondary Indexes](https://www.dynamodbguide.com/local-or-global-choosing-a-secondary-index-type-in-dynamo-db/)
* Global Vs Local Secondary Indexes
  * GSIs can be created at any time, LSIs must be created when the table is created.
  * LSI have a 10GB collection limit (partition key) - know your data!
  * LSI must use the same partition key as the base table - GSI no such limit.
  * LSIs use capacity from the base table, GSIs have their own capacity settings.
  * LSI - by default all ready are eventually consistent, ConsistentRead option can be used if required, GSIs - all reads are eventually consistent.
  * Read/Write Capacity Mode & Throughput
    * Can change twice in 24 hours.
    * Transactional read requests require two read capacity units to perform one read per second for items up to 4 KB.
    * Transactional write requests require two write capacity units to perform one write per second for items up to 1 KB.
    * RCU - Read Capacity Units
      * > 4KB
      * 1 consistent read or 2 eventually consistent reads
    * WCU - Write Capacity Unit
      * > 1KB
      * Transactional write requests require 2 WCUs to perform 1 w/s up to 1KB.
  * You may create 20 global secondary indexes and 5 local secondary indexes per table.
  * Secondary Index Attributes - specify which attributes you want to project into the index.
    * *KEYS_ONLY* - Only include the keys for the index and partition, sort values.
    * *ALL* - Include all attributes in the index.
    * *INCLUDE* - Specify which attributes to include in the index.

# Amazon DAX

* Fully Managed Caching Service for DynamoDB
* Up to 10X Performance - milliseconds to microseconds
* API Compatible with DynamoDB - No code changes, point your App at DAX
* Very good for read-heavy workloads
* Up to 10 read replicas supported
  * Can be added while the cluster is running

# DynamoDB Streams

* Time ordered sequence of item-level modification to a DDB table
* Stores in a log for up to 24 hours
* Near real-time
* Stream Specification Options
  * StreamEnabled - true/false
  * StreamViewType
    * KEY_ONLY
    * NEW_IMAGE
    * OLD_IMAGE
    * NEW_AND_OLD_IMAGES
* Integrates with Lambda > "Create Trigger" in table settings.
  * AWSLambdaDynamoDBExecutionRole
    * Access DDB and access CW Logs

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
  * Not Multi-Threaded
  * Auto detection and recovery of failed nodes
  * Multi-AZ with failover
  * Cluster Mode Disabled - 1 shard, 0-5 replicas.
  * Cluster Mode Enabled - Supports up to 250 shards (<v5.0.6), 0-5 replicas per shard.
    * Actual limits dpeend on number of shards/replicas, see [Working with Shards](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Shards.html)
  * Node or shard limit can be increased to 500 (if v5.0.6 or higher)
  * Data is presistent. Can be used as a datastore.
  * Self-service updates (must apply yourself)
  * Backup/Snapshot capabilities
  * Complex Data Types
    * Strings
    * Sets
    * Sorted Sets
    * Lists
    * Hashes
    * Bitmap
    * Geospatial indexes
  * If you require data durability, you can enable the Redis append-only file feature (AOF). When this feature is enabled, the node writes all of the commands that change cache data to an append-only file. When a node is rebooted and the cache engine starts, the AOF is "replayed." The result is a warm Redis cache with all of the data intact. [Append only files (AOF) in ElastiCache for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/RedisAOF.html)
  * It's not possible to reconfigure the cluster mode in an existing cluster. You have to create a new cluster with "cluster mode" enabled.

* Elasticache for memcached
  * Can partition data across multiple nodes
  * Auto detection and recovery of failed nodes
  * It is not persistent
  * Multi-Threaded - large nodes with multiple cores or threads
  * No Multi-AZ or read replicas - When it's gone, it's gone
  * No support for snapshots
  * Auto-Discovery feature - No reconfiguring of apps when we add a new node.
  * 1-20 Nodes in a cluster.
  * No Encryption
  * Simple Data Types
    * String
    * Objects

| | Read-Replicas | Sharding | Backup & Restore | Multi-Threaded | Encryption | Auth | Port |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Redis | Yes | Yes (Cluster Mode) | Yes | No | Yes (at rest & in-transit) | Yes (requires SSL) | 6379 |
| memcached | No | Yes | No | Yes | Yes (in-transit) | No (security via SG) | 11211 |

* ElastiCache Monitoring
  * Monitor events with ElastiCache Events
    * Failure to add node
    * Success adding node
    * Modification of an SG
  * Send a notification to an SNS topic

# Amazon DocumentDB

* DynamoDB Backups
  * Created asynchronously
  * Do not consume provisioned throughput of the table
  * Do not guarantee causal consistency across items (skew usually < 1s)
  * Cannot pause or cancel the backup operation
* Amazon DocumentDB (with MongoDB compatibility) keeps a record of events that relate to your clusters, instances, snapshots, security groups, and cluster parameter groups. This information includes the date and time of the event, the source name and source type of the event, and a message that is associated with the event.[DocumentDB Events](https://docs.aws.amazon.com/documentdb/latest/developerguide/managing-events.html)
  * Audit Logs are published to [CloudWatch Logs](https://docs.aws.amazon.com/documentdb/latest/developerguide/event-auditing.html)
* [DocumentDB Events](https://docs.aws.amazon.com/documentdb/latest/developerguide/managing-events.html)
  * Relate to...
    * Clusters - i.e. Notification, failure, maintenance, failover
    * Instances - i.e. Notification, failure, creation, maintenance, deletion, recovery, backup
    * Snapshots - i.e. backup
    * Security Groups - i.e. configuration change
    * Cluster Parameter Groups - i.e. configuration change
  * Events can be seen...
    * 24 hours in the web console
    * 14 days using the DocumentDB API/cli to view events

# Amazon Neptune

# Amazon Database Migration Service

* AWS Data Migration Service (AWS DMS) helps you migrate your data to and from widely used commercial and open-source databases, including Oracle, MySQL, and PostgreSQL. The service supports homogeneous migrations such as Oracle to Oracle, and heterogeneous migrations such as Oracle to PostgreSQL or MySQL to Oracle.

* Databases can be migrated with severall tasks, i.e. one for each table.
* Tasks can work concurrently and idependently.
* AWS DMS loads eight tables at a time by default.
* Large tables can be broken up into multiple tasks.

# Amazon Schema Migration Tool

* AWS Schema Conversion Tool (AWS SCT) makes heterogeneous database migrations easy by automatically converting the source database schema and a majority of the custom code to a format compatible with the target database.
* AWS SCT can produce a migration assessment report
  * What can and can't be converted by the tool
  * Estimates the amount of effort to rewrite/re-engineer the DB feature