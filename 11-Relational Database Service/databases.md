Databases:

Relational (SQL) vs Non-Relational (NoSQL)

Databases are systems that store and manage data. There are a number systems to chose from.  
They differ in how data is physically stored on disk, how it is managed on disk and memory and how systems retrieve data and present it to the user.

Relational:
1. Structured Query Language (SQL)(feature of most relational databases) -used to store update and retrieve data * not to be confused with Relational Database Management System
2. Structure in and between tables of data - Rigid Schema (defined in advanced before you put any data)
3. Fixed Relationships between tables
4. NoSQL - Not one single thing …. Different models
5. Generally a much more relaxed Schema
6. Relationships between tables are handled differently
7. Common examples of No SQL databases:
    1. Key-Value
        1. No Schema 
        2. No Structure
        3. Scalable 
        4. Really fast
    2. Wide Column Store
        1. Each row has a Partition Key (key1) and optionally additional keys.
        2. Second key (k2) is called  (sort or range key)
        3. Dynamo DB is an example
        4. Mix and match attributes on items
        5. No Attribute Schema
        6. Each item has to have the same key structure (portion, composite or single) 
        7. Keys have to be unique
    3. Document
        1. Generally use JSON or XML format structure
        2. Different structures can be used between documents
        3. Each document is interacted with an using unique ID value
        4. The value contents are expose to the database allowing interaction
        5. Document databases are generally use for order, collections and contact style databases.
        6. Good when you need to interact with deep attributes [nested data items within a document structure]
        7. Works well with catalogs, user profiles and content management systems.
        8. Where each document is unique and changes over time (versions)
        9. Documents may be linked together in hierarchical structures
        10. Flexible indexing so you can run powerful queries against the data the could be nested deep inside a document
    4. Column Store (Redshift - Data warehousing)
        1. Great for reports or when all values for a specific attribute (size) are required
        2. Not good for transaction style query. 
        3. Use case is taking an OLTP database and shifting that into a column based database when you want to perform analytic  and reporting.
    5. Row Store (MySQL)
            1. Ideal if you are operating with rows (adding, deleting, updating)
            2. Called (OLTP) Online Transaction Processing
    6. Graph Database
        1. Relationships between things are formally defined and stored in the database along with the data
        2. Relationship driven data - (Social Media) 
        3. Great for complex relationships
    7. ACID or BASE
        1. ACID  and BASE are DB transaction models
        2. CAP Theorem Consistency, Availability, and Partition tolerant (resilience)— Choose 2
            1. Consistency means every read to the database will receive the most recent write or get an error
            2. Availability means that every request will get an non-error response but without the guarantee that it contains the most recent write.
            3. Partition tolerance means that the system can be made of multiple network partitions and the system continues to operate. Even if there are errors and dropped messages between the nodes.
        3. ACID = Consistency
            1. Transactions are atomic, consistent, isolated, durable
            2. AWS RDS are acid databased; they  limits scaling.
            3. Atomic - All transactions are successful or none.
            4. Consistent - Transaction move the database from one valid to another -  nothing in between is allowed
            5. Isolated - if multiple transactions occur at once, they don’t interfere with each other.  Each executes as if it’s the only one.
            6. Durable - Once committed transactions are durable.  Stored on non-volatile memory, resilient to power outages or crashes.
        4. BASE = Availability
            1. Basically Available Soft STATE Eventually consistent
            2. Read and Write operation are available as much as possible but without any consistency guarantees
            3. The database doesn’t enforce consistency, this is offloaded onto the application/user
            4. If we wait long enough, reads form the system will be consistent
            5. Highly scalable and High performance.
                1. BASE means no SQL database
                2. ACID means RDS database
                3. DynamoDB normally operates in a BASE like way it offers both eventually and immediately consistent reads.  But application has to be aware.  Offers ACID transactions.
                4. No SQL or DynamoDB  mention together with ACID it is referring to Dynamo DB transaction
                5. DynamoDB transaction enable ACID but normally acts a BASE
        5. Database on Ec2
            1. Normally a bad idea
            2. You might think about databases on EC2
                1. Access to the DB Instance OS (Do you REALLY need it ?)
                2. Advanced DB Option tuning (DBROOT)
                3. …Vendor demands (delve into the justifications)
                4. DB or DB Version AWS don’t provide (niche use cases)
                5. Specific OS/DB Combination AWS don’t provide
                6. Architecture AWS don’t provide (replication/resilience)
                7. Decision makers who ‘just want it’
            3. Why you shouldn’t really
                1. Admin overhead - managing EC2 and DBHost
                2. Backup / DR Management
                3. EC2 is single AZ
                4. Features - some of AWS DB products are amazing
                5. EC2 is ON or OFF - no server less, no easy scaling (Bursty Demand)
                6. Replication - skills, setup time, monitoring and effectiveness
                7. Performance AWS invest time into optimization and features
        6. Relational Database Service (RDS)
            1. Database as a service (DBaaS)
            2. DatabaseServer as a service
            3. Managed Database Instance (1 + Databases)
            4. Multiple engines MySQL, MariaDB, PostgreSQL, Oracle, and Microsoft SQL Server
            5. Amazon Aurora — Different — 
        7. RDS High-Availability (Multi AZ)
            1. MultiZA is a feature of RDS which provisions a standby replica which is kept in sync Synchronously with the primary istance.
            The standby replica cannot be used for any performance scaling....only availability
            Backups, software updates and restarts can take advantage of MultiAZ to reduce user disruption.
        8. RDS access only via CNAME.  CNAME always points to primary.
            1. writes
            2. Disk Write to Primary
            3. Replicated to Standby
            4. Disk Write to Secondary
        9. If error occurs to primary DB
            1. The CNAME is changed by RDS to the secondary.
                1. There is some delay between fail over
        10. Key points
            1.  No Free tier -  Extra cost for standby replica
            2.  Standby can't be directly used - (only available thru cname)
            3.  60 - 120 seconds failover (Highly Available not fault tolerant)
            4.  Same region only (other AZs in the VPC)
            5. Backups taken from standby (removes performance impact)
            6. AZ outage, primary failure, manual failover, instance type change and     software patching
        11. RDS Backups and Restores
            1. RDS is capable of performing manual snapshots and automatic backups
                Manual snapshots are performed manually and live past the termination of an RDS instance.
                Automatic backups can be taken of an RDS instance with 0 (Disable) to 35 day retention.
            2. RTO vs RPO
                1. Recovery Point Objective (RPO)
                    1. Time between last backup and the incident
                    2. Amount of maximum data loss
                    3. Influences technical solution and cost
                    4. Generally lower values cost more
                2. Recovery Time Objective (RTO)
                    1. Time between the DR event and full recovery
                    2. Influenced by process, staff, tech and documentation
                    3. Generally lower values cost more
            3.     RDS Backups
                Both types use AWS Managed S3 Bucket (not visible thru S3 console) for resiliency across AZ (Resilence across the region)
                Backups occur from single DB instance if multiAZ is disable or from the Standby AZ if using multiAZ
                1. Automatic Backups
                    1. Backups occur during backup window you define
                    2. If using single AZ timing should be done when you are not actively using the database.
                    3. The window effects the RPO - time should minized the RPO window
                    4. Every 5 mins transaction logs sent to the S3 -  stores the actual data which changes inside the database -  theory RPO is 5 mins
                    5. Set retention period form 0 to 35 days
                    6. You can delete the RDS but the backups will be deleted once the retention policy time has expired.  Only way to keep is to create a final snapshot
                2. Manual snapshots
                    1. the first snapshot is FULL size of consumed data then onward it is incremental.  Only changes are backup after initial full backup
                    2. Their is a brief interrupton unless using multiAZ
                    3. Manual snapshot live forever in your account until you delete them
                    4. If you delete the RDS the snapshot still lives unless deleted
                3. Key Points
                    1. RDS Restore -  Creates a new RDS instance -  new address - applicatons need updating to use this new address
                    2. Snapshots = single point in time, creation time
                    3. Automated = any 5 minute point in time
                    4. Backups is restored and transaction logs are 'replayed' to bring the DB to the desired point in time
                    5 . Restores aren't fast - Think about RTO
            4. RDS Read-Replicas
                1. RDS Read Replicas can be added to an RDS Instance - 5 direct per primary instance
                2. They Can provide read performance scaling for the instance, but also offer low RTO recovery for any instance failure issues N.B they don't help with data corruption as the corruption will be replicated to the RR.
                3. Two main benefits are performance and availability.
                4. Asyncronous replication -  it's written fully to the primary instance first, then once it's stored on disc, it's replicated to the Read-Replicas. (Syncronous data is written to primary then at the same time writting to disk it is replicated to to the standby instance)
                5.  For the exam Synchronous replication means Multi-AZ and Asynchronous replication means Read-Replica
                6. In theory there could be small amount of lag.  between the primary instance and the Read-Replicas
                7. Read Replicas can be created in the same region or a different region
                8. Cross region Read-Replica AWS handles all the networking and fully encrypted.
                9.  5x direct read-repkicas per DB instance
                10.  Each providing an additional instance of read performance
                11. RR are the way you can scale out read capacity for a database
                12. You can configure your applications to perform read operations against the RR themselves instead of the primary instance and use the primary for write operations.
                13. Generally you can deploy RR and Multi-AZ at the same time.
                14.  Read-Replicas can have read-replicas but lag starts to be a problem.
                15. Global performance improvements
                16. No writes only reads
                17. Availability improvements:
                    1. Snapshots and Backups improve RPO
                    2. RTO are a problem
                    3. RR's offer nr. 0 RPO
                    4. RRs can be promoted quickly - low RTO
                    5. Failure only - Watch for data corruption
                    6. Read only until promoted
                    7. Global availability improvements....global resileence
                18. Key points
                    1. You can only have 5 Read-Replicas 
                    2. RR are the only technique that provide read scaling for RDS
                    3. You do not use Multi-AZ for read scaling only availability.
                    4. RR are how you scale read loads on your database but you can't scale writes.
            Amazon RDS Security
                    1. Authentication, Authorization, Encryption in Transit, Encryptoin at Rest
                    2. SSL/TLS (in Transit) is available for RDS, can be manadatory
                    3. RDS supports EBS volume encryption - KMS
                    4. Handled by HOST/EBS
                        1. As far as the RDS knows it's writing unencrypted to storage
                        2. The data is encrypted by the host RDS is running on.
                    5.  AWS or Customer Managed CMK generates data keys
                    6. Data keys used for encryption operations
                    7. encryption can't be removed
                    8. RDS MSSQL and RDS Oracle support TDE (Transparent Data Encryption)
                        1. Encryption is handled WITHIN the DB engine (not on the host)
                         2. This means less trust -  you know the data is secure from the moment it's written out to disk for the DB engine
                         3. RDS Oracle support integration with CloudHSM
                            1. Much Stronger key controls (even from AWS)( no key exposure to AWS)
                    9. Snapshots of encrypted RDS instances are also encrypted using the same key
                     10. Data transfered as replica is encrypted
                     11.  Amazon RDS IAM authentication
                        1. Configure RDS to allow IAM user authentication against a DB
                            1. On an RDS instance create a local account
                            2. Configured to allow authentication using an AWS authentication Token
                            3. IAM users and Roles attach policies.
                            4. These policies contain a mapping between that IAM entity (user or role and local RDS database user.).  Policy attached to User or Roles maps that IAM identity onto the local RDS user
                            5. Generate-db-auth-token operation for 15 minute validity (login without a password)
                            6. This is only AUTHENTICATION not authorization
                            7.  Authorizaton is still handled internally
                Amazon Aurora Architecture
                    1. Aurora is a AWS designed database engine officially part of RDS
                    2. Aurora implements a number of radical design changes which offer significant performance and feature improvements over other RDS database engines.
                    3. Aurora architecture is VERY different from RDS
                    4. Uses Clusters
                    5. A singel primary instance + 0 or more replicas -  Can be used for read operations!!!
                    6. No local storage -  uses cluster volume - faster provsion- better performance, improved availability
                    7. Replication happens at a storage level so no extra resources are consumed on the instances or the replicas duing the replicaton process.
                    8. By default the primary instance is the only instance able to write to storage and the replicas and the primary can perform read operations.
                    9. Aurora maintains multiple copies of your data in three availability zones.
                    10. All SSD based - high IOPS, low latency
                    11. Storage is billed based on what's used
                    12. High water mark - billed for the most used
                            1. Storage which is freed up can be re-used
                            2.  If you need a to reduce cost you need to provision a new storage cluster
                            3. Replicas can be add or removal without requiring storage provision
                    13.  Cluster End point (points to primary)
                    14. Reader Endpoint (load balances over replicas if there are any)
                    15. No free teir
                    16. Aururo doesn't support Micro Instances
                    Beyond RDS single AZ (micro) Aurora offers better value
                    17. Compute hourly charge per second, 10 minute minimum
                    18. Storage - GB- Month consumed, IO cost per request
                    19. 100% DB size in backups are included.
                    20. Backups in Aurora work in the same way as RDS
                    21. Restores create a NEW CLUSTER (same as RDS)
                    22. Backtrack can be used which allow IN-PLACE REWINDS to a previous point in time
                    23.  Fast clone make a new database MUCH fster than copying all the data - copy on write
            Aurora Serverless Concepts
                1. Scalable - ACU - Aurora Capacity Units
                2. Aurora Serverless cluster has a MIN and MAX ACU
                3. Cluster adjusts based on load
                4. Can go to 0 and be paused
                5. Consumtion billing per-second basis
                6. Same resilience as Aurora (6 copies across AZs)
                7. Simple to manage database instance and simpler to scale. No disruption to client data.
                Aurora Serverless - Use Cases
                    1.  Infrequently used applications
                    2.  New Applications
                    3.  Variable workloads
                    4. Unpredictictable workloads
                    5. Development and test databases
            Aurora Global Database
                1.  Aurora global databases are a feature of Aurora Provisioned clusters which allow data to be replicated globally providing significant RPO and RTO improvements for BC and DR planning. Additionally global databases can provide performance improvements for customers .. with data being located closer to them, in a read-only form. 
                2.  Replication occurs at the storage layer and is generally ~1second between all AWS regions.
                3.  Entire secondary cluster is read only
                4. Use Cases
                    1.  Cross-Region DR and BC
                    2. Global Read Scaling - low latency performance improvements
                    3. ~1 or less replication between regions
                    4. No impact on DB performance ( replication happens at the storage layer)
                    5.  Secondary regions can have 16 replicas
                    6.  Can be promoted to R/W
                    7. Currently MAX 5 secondary regions
            Aurora Mult-Master
                1.  Multi-master write is a mode of Aurora Provisioned Clusters which allows multiple instances to perform reads and writes at the same time - rather than only one primary instance having write capability in a single-master cluster
                2. Default mode is single master
                2. One R/W and 0+ Read only replicas
                4. Cluster endpoint is used to write, read endpont is used for load balanced reads.
                5. Failover takes time - replica promoted to R/W
                6. In Multi-Master mode all instances are R/W
                7.  No concept of load balance endpoints the application can directly connect to one or all instances and initiate operations.
                8.  Cluster Volume 64 TiB, 6 Replicas AZ
                9.  The application proposes the data be committed to all of the storage nodes in that cluster.
                10.  Each node that makes up a cluster either confirms or rejects the proposed change, (Quorum)
                11. If a quorum of nodes agree it is allowed to write that data. (or reject)
                12.   That change is replicated to other nodes in the cluster (in-memory cache for fast access) doesn't need to retrieve it from storage.
            Database Migration Service (DMS)
            1.  The Database Migration Service (DMS) is a managed service which allows  for 0 data loss, low or 0 downtime migrations between 2 database endpoint. The service is capable of moving databases INTO or OUT of AWS.
            2. Runs using a repliation instance
            3.  Source and Destination Endpoints point at....
            4.  Source and Target Databases
            5.  One endpoint Must be on AWS
                1. Jobs can be 
                    1.  Full load (one off migration of all data),  
                    2.  Full load + CDC for ongoing replication which captures changes or CDC only
                    3. CDC only (if you want to use an alternative method to transfer the bulk DB data ... such as native tooling)
                    7. Schema Conversaion Tool can assist with Schema Conversion (SCT)




            











