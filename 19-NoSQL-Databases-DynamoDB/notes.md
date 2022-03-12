## Amazon DynamoDB (DDB)
                NoSQL Public Database as a Service (DBaaS) - Key/Value  &Document
                Accessable anywhere with access to the publicv endpoints of DynamoDB. Public internet, VPC with either a internet gateway or a gateway VPC endpoint.

                no self-managed servers or infrastructure
                                It's not like RDS or Aurora or Aurora Serverless, which are Database Server as a Service products.  You get the database itself delivered as a service, and this reduces the complexity and admin overhead
                Manual /  Automatic provisioned performance IN/OUT or On-Demand
                Highly Resilient ... across AZs and optionally global

                Should be call database table as a service:
                    Table is a grouping of ITEMS with the same PRIMARY KEY
                    Simple (Partition) or Compsite
                    (Partition & Sort) PRIMARY KEY
                    Each item MUST have a unique value for PK and SK.
                    (DDB has no rigid attribute schema)
                     ITEM MAX 400KB

                Capacity (speed) is set on a table
                (Write) 1 WCU = 1KB per second
                (Read) 1 RCU = 4KB per second

DynamoDB On-Demand Backups
                    Full copy of Table until Removed
                    Restore - Same or Cross Region
                    With or Without Indexes
                    Adjust Encryption settings

Consideration:
                    NoSQL ... preference DynamoDB in the exam *
                    Relational Data ... generally NOT DynamoDB *
                    Key/Value.. preference DynamoDB in the exam *
                    Access vis console, CLI, API ... 'NO SQL'
                    Billed based RCU, WCU, Storage and features

Point-in-time Recovery
                    Not enabled by default - 1 second granularity
                    Continuous record of changes allow replay to any point in the window - 35 day recovery window

DynamoDB Operations, Consistency and Performance
                    Reading and Writing
                    on Demand - unknow, unpredictable, low admin
                    on-Demand - price per million R or W units
                    Provisioned ... RCU and WCU set on a per table basis
                    Every operation consumes at least 1 RCU/WCU *
                    1 RCU is 1 x 4KB read operation per second *
                    1 WCU is 1 x 1KB operation per second *
                    Every table has a RCU and WCU burst pool (300 seconds)
            Query
                    Have to pick at least one partition value
                    Query accepts a single partion key and optionally a Sort Key or range (composite primary key)
                    capacity consumed is the size of all returned items
                    Further filtering discards data - capacity is still consumed.
                    Can ONLY query on PK or PK or SK
            Scan
                    moves through a table consuming the capacity of every ITEM.  You have complete control on what data is selected , any attributes can be used and any filters applied but can SCAN consumes capacity for every ITEM scanned through.

Consistency Model
            Eventual consistency
                    reads check 1/3 nodes could be unlucky with stale data if a node is checked before replication completes.

            
             Strongly consistent 
                    reads connect to the leader node to get the most up-to-date copy of data.

            WCU calculation
            if you need to store 10 ITEMS per second ... 2.5K average size per ITEM
                    Calculate WCU per item .. Round up (ITEM SIZE/ 1KB) (3)
                    Multply by average number per second (30)
                        = WCU 30

            RCU calculation
             if you need to store 10 ITEMS per second ... 2.5K average size           
             Calculate RCU per item Round UP (ITEM SIZE / 4KB) (1)
             MULTIPLY by average read ops per second (10)
                        = Strongly Consistent RCU Required (10)
            50 percent of strongly consistent = Eventually Consistent RCU Required (5)
            
  DynamoDB - Streams & Lambda Triggers
            DynamoDB Streams are a 24 hour rolling window of time ordered changes to ITEMS in a DynamoDB table

            Records INSERTS, UPDATES, and DELETES

            Streams have to be enabled on a per table basis , and have 4 view types

            KEYS_ONLY

            NEW_IMAGE

            OLD_IMAGE

            NEW_AND_OLD_IMAGES

            Streams => Triggers
            Event stream architecture
            ITEM changes generate an event
            That event contains the data which changed
            A action is taken using that data
            AWS = Streams + Lambda
            Reporting & Analytics
            Aggregation, Messaging or Notifications

            Lambda can be integrated to provide trigger functionality - invoking when new entries are added on the stream.

DynamoDB Local and Global Secondary Indexes
            Query is the most efficient operation in DDB
            Query can only work on 1 PK value at a time...
            ... and optionally a single, or range of SK values
            Indexes are alternative views on table data
            Different SK (LSI) or Different PK and SK (GSI) *
            Some or all attributes (projection) *

Local Secondary Indexes (LSI)
            LSI is an alternative view for a table
            MUST be created with a table *
            5 LSI's per base table
            Alternative SK on the table (same partion key) *
            Shares the RCU and WCU with the table *
            Attributes - ALL, KEYS_ONLY and INCLUDE

Global Secondary Indexes (GSI)
            Can be created at any time
            Default limit of 20 per base table
            Alternative PK and SK
            GSI's have their own RCU and WCU allocations
            Attributes - ALL, KEYS_ONLY and INCLUDE
            GSI's are always eventually consistent, replication between base and GSI is Asynchronous

DynamoDB Global Tables
            DynamoDB Global Tables provides multi-master global replication of DynamoDB tables which can be used for performance, HA or DR/BC reasons.
            
            Tables are created in multiple regions and added to the same global table (becoming replica tables)

            Last writer wins is used for conflict resolution
            Reads and Writes can occur to any region
            Generally sub-second replication between regions
            Strongly consistent reads ONLY in the same region as write
            Global eventual consistency same-region eventual or strongly consistent.

            Provides Global HA and Global DR/BC

DynamoDB Accelerator (DAX)
            DynamoDB Accelerator (DAX) is an in-memory cache designed specifically for DynamoDB. It should be your default choice for any DynamoDB caching related questions.
             
             Primary NODE (Writes) and Replicas (READ)
             Nodes are HA .. Primary failure = election
             In Memory cache - scaling .. Much faster reads, reduced costs
             Scale UP and Scale OUT (Bigger or More)
             DAX Deployed WITHIN a VPC
             supports read caching of items and query/scan results
             supports write-through and read caching
             Good for read heavy workloads

Amazon Athena
            Amazon Athena is serverless querying service which allows for ad-hoc questions where billing is based on the amount of data consumed.
            Athena is an underrated service capable of working with unstructured, semi-structured or structured data.
            
            Schema-on-read - table-like translation
            Original data never changed - remains on S3

            Schema translates data => relational-like when read
            "Table" are defined in advance in a data catalog and data is project through when read.  It allows SQL like queries on data without transforming source data.  Output can be sent to visualization tools.

            Athena can directly read many AWS data formats such as CloudTrail, ELB Logs, and Flow Logs.

ElasticCache
            In-memory database .. high performance
            Managed Redis or Memcache .. as a service
            Can be used to cache data - for READ HEAVY workloads with low latency requirements *
            Reduces database workloads (expensive) *
            Can be used to store Session Data (Stateless Servers)
            REQUIRES APPLICATION CODE CHANGES!! *

            its useful for read heavy workloads, scaling reads in a cost effective way and allowing for externally hosted user session state.

            Memcached 
                    Support simple data structures
                    No Replication
                    Multple Nodes (Sharding)
                    No backups
                    Multi-thread

            Redis
                    Advanced Structures
                    Multi-AZ
                    Replicaton (Scale Reads)
                    Backup and restores
                    Transactions

Amazon Redshift Architecture
            Redshift is a column based, petabyte scale, data warehousing product within AWS

            Its designed for OLAP products within AWS/on-premises to add data to for long term processing, aggregation and trending.

            Different from OLTP (online  transaction processing) (row/transaction)

            Pay as you use ... similar structure to RDS
            Direct Query other DBs using federated query
            Integrates with AWS tooling such as Quicksight
            SQL-like interface JDBC/ODBC connections

            Server based (not serverless)
            One AZ in a VPC - network cost/performance
            Leader Node - Query input, planning and aggregation
            Compute Node - performing queries of data
            VPC security, IAM Permissions, KMS at rest encryption, CloudWatch monitoring
            Redshift Enhanced VPC routing - VPC Networking *

Redshift Resilience and Recovery
            Automatic incremental backups occur every - 8 hours
            or 5GB of data and by default have a 1-day retenion -
            configurable up to 35 days

            Manual snaps can be taken at any time and deleted by an admin as required.
            Redshift backups into S3 protect against AZ failure
            Can be configured to copy snapshots to another region for DR with a seperate configurable retention period.


            








                



