# SAA-Database & Analytics

>数据库是作为架构师最重要的能力之一，候选方案很多，依据不同场景来选择。主要涉及的服务有：RDS、DynamoDB、Elasticache、Redshift、Kinesis等

## 知识点(易)

- 数据库选择
  - 这是常考的概念，有几个原则可以参考：根据数据类型来选（结构化&非结构化）、根据应用场景选（Transaction业务&Analytics业务）、根据扩展性和可用性来选（scalability&HA）、根据数据量来选、根据读写性能来选
- HA & Repica & cross-region
  - 常见题，比较容易

## 知识点(中)

- DynamoDB
  - 数据库里面出现频率最高
  - 由于是AWS自己的服务，很多场合下都被AWS推荐使用
  - table & index & DAX最好也了解下
- Aurora
  - 同样是AWS自己的服务，被推荐使用
  - 在和其他RDS一起出现的时候，aurora往往是优选
- Elasticache
  - 多数用在解决DB的读性能问题
  - 常和replica一起出现
- Redshift & Kinesis
  - 数据分析场景下使用
  - 多数情况下都作为干扰项出现
  - 掌握要点即可

## 知识点(难)

- Backup & DR
  - 备份和容灾偶尔会出现，比较难一些

## 要点记录

- RDS
	- Aurora, Mysql, MariaDb, PostgresSQL, Oracle, SQL Server
	- RDS storage use EBS volume for database and log storage and automatically strips across multiple EBS volumes to enhance performance depending on required storage amount
	- underlying volume types: General Purpose SSD Storage and Provisioned IOPS SSD Storage
	- RDB turns on automated backup of db instance to S3 by default. backup retention period is up to 35 days.
	- read replica for scalability
		- for read-heavy database workloads
		- available for all but SQL Server
		- for mysql, mariadb, postgresql, orocal, RDS create source instance’s snapshot and then use engine’s native asynchronous replication to update the read replica
		- up to 5 read replicas to each instance
		- replica is read-only
		- can be within an AZ, cross-AZ, cross-region
		- can be promoted to a standalone database instance
		- replica is not backup configured by default
	- multi-AZ for high availability and durability
		- synchronous replication
		- 2 AZs in a same region
		- automated backups are taken from standby
		- automatically failover to standby when a problem is detected
	- encryption at rest using KMS keys; support SSL encryption in transit
	- RDS provides CloudWatch metrics; can notify events through SNS; integrate with AWS Config to audit configuration changes
	- When you delete a DB instance, all automated backups are deleted and cannot be recovered. Manual DB snapshots of the instance are not deleted.
	- [Best practices](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Recommendations.html)
- Aurora
	- simplicity and cost-effectiveness
	- auto-scales up to 64TB per db instance
	- automatically replicates your storage 6 ways, across 3 AZs
	- mysql and postgresql compatible with 5x performance of mysql and 3x performance of postgresql
	- employs an SSD-backed virtualized storage layer purpose-built for database workloads. Amazon Aurora replicas share the same underlying storage as the source instance, lowering costs and avoiding the need to copy data to the replica nodes
	- Aurora stores copies of the data in a DB cluster across multiple Availability Zones in a single AWS Region, regardless of whether the instances in the DB cluster span multiple Availability
	- When you create Aurora Replicas across Availability Zones, Amazon RDS automatically provisions and maintains them synchronously.
	- multiple read replicas
	- Any Aurora Replica can be promoted to become primary without any data loss and therefore can be used for enhancing fault tolerance in the event of a primary DB Instance failure
	- Aurora endpoints
		- cluster endpoint
			- connect to primary db instance
			- the only instance that can perform write operations
		- reader endpoint
			- connect to one of the available replicas
		- customer endpoint
			- you choose a set of dbs
			- aurora performs load balancing in the group
		- instance endpoint
			- connect to a specific db instance
- DynamoDB
	- key value and document database
	- for mission-critical workloads
	- low latency at any scale
	- full managed database with built-in security, backup and restore, and in-memory cacheing for internet-scale application
	- data is automatically replicated across multiple AZs in a AWS region, providing build-in high availability and data durability
	- global tables is multi-region, multi-master
	- support petabytes of data and tens of millions of read write request per second
	- offers encryption at rest
		- AWS KMS store keys
		- 2 kinds of CMK to encrypt table: AWS owned CMK, AWS managed CMK
	- Tables, items, attributes; 2 types of primary keys: partition key, partition key and sort key; secondary indexes: global secondary index, local secondary index
	- DynamoDB streams capture data modification events in DynamoDB tables; can integrate with AWS lambda to create a trigger-code; also can be used for data replication, data analysis and much more; is an ordered flow of changes to items; can not be accessed using VPC Endpoint
	- consistency: eventually consistent read (default), strongly consistent read
	- read/write capacity mode
		- on-demand: pay per request
		- provisioned (default): you specify max request rate for predictable traffic or control cost; if provisioned capacity is exceeded, requests above capacity will be throttled and failed with a HTTP 400 code
		- write unit is 1KB and read unit is 4KB
	- [Best practice](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)
		- keep item small size
		- store metadata in DynamoDB and large BLOBs in S3
		- use table per day, week, month etc for storing timing series data
		- use conditional or optimistic concurrency control (OCC) update
		- avoid hot keys and hot partitions
		- avoid scan operation
	- DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory cache for DynamoDB that improves 10x performance from milliseconds to microseconds
	- DynamoDB Auto Scaling is primarily used to automate capacity management for your tables and global secondary indexes.
	-   

- ElastiCache
	- in-memory cache with sub-milliseconds response time
	- do not provide durability
	- use case: gaming, ad-tech, financial service, healthcare, IoT apps
	- write and memory scaling is supported with sharing. replicas provide read scaling
	- redis
		- Maximize availability by leveraging Multi-AZ with automatic failover functionality. You can take advantage of multiple AWS Availability Zones to gain availability, and scale beyond the read capacity constraints of a single node. In case of primary node loss, we will automatically detect the failure and failover to a read replica to provide higher availability without the need for manual intervention.
		- Supporting up to 250 nodes and shards, you can scale up to 155.17 TiB (170.6 TB) of in-memory data with 48.6 million reads and 9.7 million writes per second.
	- memcached
- Redshift
	- fully managed, fast and powerful, petabyte scale data warehouse service
	- provide fast querying capabilities over structured data using familiar SQL-based clients and business intelligence (BI) tools using standard ODBC and JDBC connections
	- supports VPC, SSL, AES-256 encryption and Hardware Security Modules (HSMs) to protect the data in transit and at rest
	- provides columnar storage
	- Redshift only supports Single-AZ deployments and the nodes are available within the same AZ, if the AZ supports Redshift clusters
	- Workload Management (WLM)
		- manages query concurrency and memory allocation
		- define the number of query queues that are available, and how queries are routed to those queues for processing
		- part of parameter group configuration
	- By using Enhanced VPC Routing, you can use standard VPC features, such as VPC security groups, network access control lists (ACLs), VPC endpoints, VPC endpoint policies, internet gateways, and Domain Name System (DNS) servers
- Kinesis
	- realtime data capture and analytics
	- synchronously replicates data across 3 AZs in one region providing high availability and high durability
	- stores stream data up to 24 hours which can be raised up to 7 days
	- provides ordering of records
	- support multiple consumers
	- has four distinct services under it: Kinesis Data Firehose, Kinesis Data Streams, Kinesis Video Streams, and Amazon Kinesis Data Analytics
	- kinesis data stream
		- an ordered sequence of data records meant to be written to and read from in real-time
		- Data records are therefore stored in shards in your stream temporarily.
		- A Kinesis data stream stores records from 24 hours by default to a maximum of 168 hours.
		- requiring manual scaling and provisioning
	- kinesis data firehouse
		- Amazon Kinesis Data Firehose is the easiest way to load streaming data into data stores and analytics tools
		- scaling is handled automatically
		- capture, transform, and load streaming data into S3, Redshift, ES, and Splunk, enabling near real-time analytics and insights
