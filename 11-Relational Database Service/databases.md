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
