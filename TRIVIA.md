# This section details specific information that I want to remember for the Exam

* Read operations such as GetItem, Query, and Scan provide an optional ConsistentRead parameter. If you set ConsistentRead to true, DynamoDB returns a response with the most up-to-date data, reflecting the updates from all prior write operations that were successful. Strongly consistent reads are only supported on tables and local secondary indexes. Strongly consistent reads from a global secondary index or a DynamoDB stream are not supported. [DynamoDB Read Consistency](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)
* GetItem provides an eventually consistent read by default. [DynamoDB GetItem](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_GetItem.html)
* PostgreSQL - `rds.log_retention_period` default 4,320 (3 days)
* In RDS we do not have access to the underlying bucket that stores Automated Snapshots. So we cannot set CRR (Cross-Region Replication).
* RDS - You can have up to 100 manual snapshots per Region. [RDS Backups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html)
* When you export a DB snapshot, Amazon RDS extracts data from the snapshot and stores it in an Amazon S3 bucket. The data is stored in an Apache Parquet format that is compressed and consistent.[Exporting DB Snapshot data to Amazon S3](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ExportSnapshot.html)
* RDS - MSSQL - the Active Directory Connector alone is not enough to set up Windows Authentication to your SQL Server database. You have to launch an AWS Managed Microsoft AD with a trust relationship with the on-premises Active Directory to meet the specified requirement. [AWS Managed AD with RDS for SQL Server](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_SQLServerWinAuth.html)
* Database Activity Streams, currently supported for Amazon Aurora, provides a real-time data stream of the database activity in your relational database. When integrated with 3rd party database activity monitoring tools, you can monitor and audit database activity to provide safeguards for your database and meet compliance and regulatory requirements. [RDS Database Actitity Streams](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/DBActivityStreams.Overview.html)
* For applications that need to read or write multiple items, DynamoDB provides the BatchGetItem and BatchWriteItem operations. Using these operations can reduce the number of network round trips from your application to DynamoDB. In addition, DynamoDB performs the individual read or write operations in parallel. Your applications benefit from this parallelism without having to manage concurrency or threading. [Working with items and attributes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html)
* RDS PostgreSQL - The event *IO:XactSync* indicates that the instance is spending time waiting for the Aurora storage subsystem to confirm that transaction data was processed. [IO:XactSync](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/apg-waits.xactsync.html)
  * Indicates network saturation or CPU pressure.
* Prevent an RDS from being accessed outside the VPC
  * Ensure "Public acccessability" on the RDS instance is disabled
  * Create a SG that allows traffic on port 3306 only from EC2 Instances.
  * Ensure DB subnet group consists of two private subnets.
* Amazon Netptune
  * Vertex file - ~id column header required.
  * Edge file - ~id,~from, and to column headers required.
* AWS Aurora - Cannot make an encrypted copy of a snapshot while standard RDS can.
* Aurora DB Cluster - up to 15 read replicas. Can be Auto-Scaled by provisioning and deprovisioining replicas.
* An AWS Snowball Edge device can hold up to 100 TB of data.
* Amazon Redshift uses the Amazon Simple Notification Service (Amazon SNS) to communicate notifications of Amazon Redshift events.
* ElastiCache for Redis clusters - AUTH can only be enabled if in-transit encrption was enabled during creation.
* Amazon RDS Performance Insights is configured on a per-instance level. 
* It is possible to move and RDS DB Instance to a new VPC
  * Should be single-AZ only (can downgrade, move and then convert back to multi-az)
  * Must delete read-replicas (can redeploy after move).
  * Subnets we want to migrate to must be in the same AZ as the source.
  * You cannot move the VPC of an Aurora Cluster/Instance in this way.
* Aurora PostgreSQL has a [Cluster Cache Management](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraPostgreSQL.cluster-cache-mgmt.html) feature to avoid cold-starts after failover. This feature requires the same db instance types.
* Reboot with failover option is suppoorted by RDS but not Aurora.
* DynamoDB - when a table is restored we must reapply IAM Policies and role, TTL Settings and Auto-Scaling POlicies.
* DynamoDB - Strongly consistent reads are constantly passed through DAX to DynamoDB.
* DynamoDB - Items in a table can only be up to 400KB.