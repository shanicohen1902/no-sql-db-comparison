# no-sql-db-comparison

# Select DB – how to?

[NoSQL Db characteristics](#nosql-db-characteristics) \
[NoSQL Database Types](#nosql-database-types) 	\
[MongoDB](#mongodb)	\
[Redis](#redis) \
[Elasticsearch](#elasticsearch) \
[Cassandra](#cassandra) \
[CouchBase](#couchbase)


# NoSQL Db characteristics

There are many NoSQL solutions around, each one with its own strengths and weaknesses, so the following must be taken with a grain of salt. 
 - Semi-structured/ unstructured schema.   
If your data requirements aren’t clear at the outset or if you’re dealing with massive amounts of unstructured data, you may not have the luxury of developing a relational database with clearly defined schema. Enter non-relational databases, which offer much greater flexibility than their traditional counterparts. Think of non-relational databases more like file folders, assembling related information of all types. If a WordPress blog used a NoSQL database, each file could store data for a blog post: social likes, photos, text, metrics, links, and more.
 - Limited transactions support (different for each product). 
 Transactions require ACID properties of how DBs perform user operations. ACID restricts how scalability can be improved: most of the NoSQL tools relax consistency criteria of the operatioins to get fault-tolerance and availability for scaling, which makes implementing ACID transactions very hard.
 - Scale horizontally ability. 
 NoSQL databases are designed to be scaled across multiple data centers / clusters. They usually try improving scalability of the data store by distributing data processing. 
 - Fast – high performance.  
no joins - read or write to one table. The process of multiple data across tables calls denormalization.  
Many NoSQL databases rely on denormalization and try to optimize for the denormalized case. For instance, say you are reading a blog post together with its comments in a document-oriented database. Often, the comments will be saved together with the post itself. This means that it will be faster to retrieve all of them together, as they are stored in the same place and you do not have to perform a join.
 - Querying language – not SQL!
 - Capabilities to store large volume of data - scale out
 - In cases that aggregation is natural , the development make easier.
 - Database denormalization - Query first approach. design our tables for queries.
**We need to understand the toolset for each Database. Find a solution that could answer our problem.** 
## Considerations of choosing Database:
Requirements - how do we need to read the data. Do we need schema type?
 - Non-functional requirements. 
 - Performance
 - Scale – data volume + load
 - Consistency vs Availability
 - Cost – consider software cost + maintenance
  - Integration considerations – Existing technologies that need to integrate. 
 - Existing dev team skills
 - CAP – choose only two: AP/AC
	 - Consistency
   - Availability
   - Partition tolerance
On distributed NoSQL databases, basically you need to choose between AP to CP. Partition tolerance is a basic requirement from distributed system.
 - Keep it simple…
# NoSQL Database types
[Key Value](#key-value-store)\
[Document Store](#document-store-database)\
[Column-oriented](#column-databases)\
[Graph](#graph-databases)
### Key-value store
 - Store simple data, accessible through a key
 - Read/write by key. The data can be from many types – Json/image/Object/Text
 - Simple schema
 - High performance and scalability
 - No complex queries involving multiple keys or joins
 - Caching feature
When to use?  The data that might be represented by a key-value like configuration, flags, Session management, User preferences\
When to avoid? When there is need to interact with the data itself.\
Examples – Redis, Amazon DynamoDB, Aerospike
### Document store Database
Similar to key-value.  Store data in structured formats called documents, often using formats like JSON, BSON, or XML.
 - The content stored in the document can be queried and analyzed.
 - Flex schema
 - Content indexes support. Data can be queried, not only the kay.
 - Easy to develop when aggregation is needed
 - If the data model needs to change, only the affected documents need to be updated 
 - Horizontally Scalable
 
![column wide](https://user-images.githubusercontent.com/58383975/97004758-2f46df80-1546-11eb-8a87-bffd4cdb5758.png)
 
Examples – MongoDB, CouchDB

**Document Design Considerations**\
When documents are initially designed, performance and scalability should be considered
 - small number of big documents.\
potentially allows information to be read or written in a single operation. This improve  atomicity and scalability, due to a fewer relations between independent objects. 
 - large number of simple documents. Appropriate when data-size needs to be kept small, in order to reduce latency. Documents can refer to each other by key.

### Column Databases
A column-oriented database stores columns sequentially. Each column will be stored in sequential blocks. Good for analytical queries that perform aggregate operations (column actions).\
When there is a need for retrieval a few columns from a table with many columns, Row-Oriented Database has to skip over unnecessary data. 
 - Store & analyze big Data sets
 - Columns data stores together
 - Quick at aggregate queries (sum, average, min, max, etc.). We read only the columns that uses in the query 
 - The data can be sorted/indexed 
 - Adding column – easy
 - Single row operations are slow (use batch, work with bulks). Saves storage: no need to define null/repeated data

Examples- Cassandra, InfiniDB, HBase

### Graph Databases
Store and querying relationships between objects. Logical structure represents data relationships. \
Edge - relationship. Edge have a direction.\
Vertex - object\
 - ACID
 - Graph DB does not represent any aggregation. 
 - Relations can be many to many (no extra table)
 - Properties - On both edges and vertex, additional information can be stored (similar to fields). Flexible schema.
 - Add new edge is easy
 - Finding vertices – based on key or properties
 - Traversals –
  - To other vertices
  - To edges
  - Filtering with traversals
Examples – Neo4j, Amazon Neptune

# MongoDB
Type: Document storage

MongoDB is a NoSQL database, uses JSON-like documents with optional schemas. It designed for ease of development and scaling.

Mongo saves BSON (like JSON) as data. 
Flex schema 
Ability to querying the data itself (not only get it)
Index support – fields/combination of fields/ special indexes. (obviously, only one can use for sharding). Automatically generated or provided by the developer. Must be unique
Mongodb supports all CRUD
More options – aggregation capabilities, bulk
Good for store a lot/big of documents and analyze them
License: AGDL

Mongos – the router of Mongo. Responsible for routing the queries to the suitable shard. Mongos can be replicated 
Mongod – the primary daemon process. It handles data requests, manages data access, and performs background management operations. (Configuration server)

### Sharding
By hash (better distribution) /by range (when need to query by range). The key couldn’t be changed!
Auto sharding by Mongos - when a chunk size limits the threshold, mongo will immigrate data to another shard

### Clustering

Single Master Architecture
 Auto re-election
 Asynchronous replication

**Write path:**
To the primary node.  Asynchronous replication to the slave\
Default - Success writing when the master succeeded. Option – write also to the majority/number of nodes.
**Read path:**
By default, clients read from the primary. when the primary fails, the system unavailable. 


### CAP – CP

(Default configuration)\
Consistency - Reads and writes from master. to ensure consistency.
Availability - when master falls system unavailable till new master re-elected (availability infected). 
Partition tolerance - obviously…

### Transactions
Starting in MongoDB 4.0, multi-document transactions are available for replica sets.
Atomic - Multi-document transactions are atomic. A transaction will not commit some of its changes while rolling back others.

  ![mongo sharding](https://user-images.githubusercontent.com/58383975/97004874-58677000-1546-11eb-887c-ca1d73eb7498.png)

# Redis
type: key-value\
Redis is an open source , in-memory data structure store, used as a database, cache and message broker.
Cache or database?\
Cache – redis can be used as distributed or in memory\
Primary store -Data can be persist to the disk to make redis safer (but also slower - trade-off)\
Optional the data loads to the disk once in a while (snapshots - .rdb files), or saves to the log each change (AOF – Append only files).  Saves the commands log is more human friendly but its uses more space and expense performance. Save a snapshot is more data loss risky + slows the system when the save process works. Both/neither options can be used\
Restore data from disk: If AOF used, by executing all the commands from it.
Restore the data from a snapshot is faster\
Supports different data structures – string, hashes, list, sets, sorted set.
Bucketing – store multiples key-value couples in one hash (less key overhead)\
Can be used as a pub-sub

  	Main point:  Fast!
License: BSD
	



### Transactions
Isolation - A basic transaction provides the opportunity for one client to execute multiple commands without other clients interrupt them. 
Atomic - Either all of the commands or none are processed\
Locking - Optimistic locking\
Optimistic – check and set, pessimistic – locks

### Replication (by Redis cluster)
For creating a distributed environment of Redis you have 2 options - Redis Sential (does not distribute data, only prevents failovers) and Redis Cluster\
Redis Cluster is a complete clustering solution. 
Redis Cluster principles:\
Master-Slave Architecture \
The client can connect any node, the node responsible to route the query if it can’t handle it to the correct node (Asynchronous replication)
 Auto healing.  There is a quorum that needs to agree on a master election and part of the system may become unavailable till the reelection ends\
Cluster bus - Every node is connected to every other node in the cluster using the cluster bus. The Cluster bus is used by nodes for failure detection, configuration update and failover authorization

**Write path:**
To the master. Asynchrony replicates to slaves. 
A master failure before asynchronous streaming of data to a slave would lead to data loss. Mechanisms such as multipath writes and disk replication can be used to prevent it.
There is an option to enhance consistency by ‘WAIT’ – success only after replicating to other nodes. 
***Read path:***
From master. There is an option to scale reads by read-only nodes. Read-only nodes can accept reads and may retrieve stale data (eventual consistency).
### Partitions 

Redis uses “Hash slots” - mapping (Similar to consistent hashing)
### CAP (default configuration)

Redis Cluster was designed as a general solution for high availability and horizontal scalability for all users of the database. As such, it was designed from the ground up with the major value additions to Redis in mind: performance and a strong data model. Because of this, Redis Cluster implements neither true availability nor consistency of the CAP theorem.

Availability – There is a quorum that needs to agree on a new master when electing a new one. At that time, part of the system may become unavailable
Consistency – It is possible to lose data if a failure occurs after a master has acknowledged a write but before replication has completed.
https://www.credera.com/blog/technology-insights/open-source-technology-insights/an-introduction-to-redis-cluster/
https://redisessentials.com/ - Chapter 9

***Lucene search concept:***
Instead of searching the text directly, it searches an index instead.
Index: an index contains a sequence of documents. 
Document: A document is a sequence of fields. 
Inverted index: Instead of pages -> word, word -> pages
Search: title=”art” -> search for all the documents 
TF-ITF for relevance 

  ![lucene terms](https://user-images.githubusercontent.com/58383975/97004885-5bfaf700-1546-11eb-809e-1bccf38d722e.png)

# Elasticsearch
Search engine based on Lucene.
Started as horizontal scalable Lucene. 
Very fast for search operations. 
Not just for text search! Can handle structured/aggregated data\
Part of the Elastic Stack – Kibana (UI Visualization), Logstash Beats (Streaming data into Elastic), X-Pack (Security, Alerting, Monitoring)\
Interface - Client API/Restful API + JSON/Analytic tools (like Kibana)\
**Lucene terms:**
Document - The stored document. (document is equivalent to a row in RDBMS)\
Field - A document contains a list of fields, or key-value pairs. The value can be a simple (scalar) value (eg a string, integer, date), or a nested structure like an array or an object.\
Term - A term is an exact value that is indexed in Elasticsearch. Terms can be searched for using term queries.\
Index – Like a DB table. Contains inverted indexes that let you search across all documents. Manages as a collection of shards - an index is a logical namespace that points to primary and replica shards.\
Type – type defines the schema and mapping shard by documents that represent the same thing. (A log entry, an article…). In Elastic 6, only one type is allowed per index.

  ![lucene terms](https://user-images.githubusercontent.com/58383975/97004991-7c2ab600-1546-11eb-909f-32074b852036.png)

Licence: Apache
### Partitioning
A shard is a single Lucene instance. It is a low-level “worker” unit which is managed automatically by Elasticsearch.\
By default, an index has one primary shard. You can specify more primary shards to scale the number of documents that your index can handle.\
Elasticsearch distributes shards amongst all nodes in the cluster, and can move shards automatically from one node to another in the case of a node failure, or the addition of new nodes.\
Elastic routs the documents based on ID hashing. This value can be overridden by specifying a routing value.
### Replication
Single master Architecture\
Automatic failover. Elastic selects a new primary\
**Read path** - Round robin requests among nodes. any node can handle read requests.\
**Write path** – to the Primary. When you index a document, it is indexed first on the primary shard, then on all replicas of the primary shard.
Scale writes - The number of primary Shards cannot be changed later. It is difficult to scale writes. In the worst-case – re-index.\
Read heavy is a good case – Add more replicas.

  ![elasticsearch topology](https://user-images.githubusercontent.com/58383975/97004999-7f25a680-1546-11eb-9b64-e1d04df75948.png)

# Cassandra
type: column-oriented DB\
Apache Cassandra is an open-source, distributed, NoSQL database.\
It presents a partitioned wide column storage model with eventually consistent semantics.
License: Apache\
**Cassandra makes the following guarantees:**
 - High Scalability - nodes can be added/removed as needed. Linear throughput increasing when adding nodes
 - High Availability -  fault-tolerant storage system
 - Durability - guarantees data durability by using replicas
 - Eventual Consistency of writes to a single table
 - Lightweight transactions with linearizable consistency (Isolation)
 - Batched writes across multiple tables are guaranteed to succeed completely or not at all
 - Secondary indexes are guaranteed to be consistent with their local replicas data\
  	Main point:  Store huge dataset in “almost SQL”
Query Language: CQ,  an SQL-like language.\
***Cassandra terms:***
  - Keyspace: defines how a dataset is replicated, for example in which datacenters and how many copies. Keyspaces contain tables. 
 - Column Family (Table): defines the typed schema for a collection of partitions. Cassandra tables have flexible addition of new columns to tables with zero downtime. Tables contain partitions, which contain columns.
 - Partition key: a key all rows in Cassandra must-have. All performant queries supply the partition key in the query.
 - Row: a collection of columns identified by a unique primary key.
 - Column: A single datum with a type that belongs to a row.
 
  ![cassandra terms](https://user-images.githubusercontent.com/58383975/97005000-7fbe3d00-1546-11eb-81b7-ebe558e32ee3.png)

In Cassandra, users can safely add columns to existing Cassandra databases while remaining confident that query performance will not degrade.\
Use at: Netflix, eBay, GitHub, Instagram, Reddit, and more…
 
### Partitioning
Cassandra uses consistent hashing for distributing the data across the nodes.
The key of the partitioning calls partition key and it doesn’t have to be unique. For example, If I will want in the future to get all the employees with car = ‘BMW’,  I can choose car type as a key, and guarantee that those employees will stay on the same node. 
### Replication
Replication factor - defines how many replicas.\
Rack - a cluster of connected machines in a data center. Data Center contains multiple racks. The rack contains one or more nodes.\ 
**Replication strategy - defines which nodes the data replicated:**\
1. Simple Strategy - For any keyspace, define how many replicas.\
For example, if we have an eight node cluster, and a replication factor of 3, then to find the owning nodes for a key we first hash that key to generate a token (which is just the hash of the key), and then we “walk” the ring in a clockwise fashion until we encounter three distinct nodes, at which point we have found all the replicas of that key.
 
 ![cassandra network topology](https://user-images.githubusercontent.com/58383975/97005004-80ef6a00-1546-11eb-9986-ab1f0428f6ee.png)

2. Network topology strategy - allows a replication factor to be specified for each datacenter in the cluster. It also attempts to choose replicas within a datacenter from different racks to maximize availability. If the number of racks is greater than or equal to the replication factor for the data center, each replica is guaranteed to be chosen from a different rack. 

 ![cassandra network topology](https://user-images.githubusercontent.com/58383975/97005005-80ef6a00-1546-11eb-9173-c8c1f2e3c89f.png)

**Consistency Levels**
In Cassandra, you choose consistency level which allows the operator to pick reads (R) and writes (W) behavior without knowing the replication factor.\
Generally, writes will be visible to subsequent reads when the read consistency level contains enough nodes to guarantee a quorum\ intersection with the write consistency level.\
Consistency levels - https://cassandra.apache.org/doc/latest/architecture/dynamo.html?highlight=rack#tunable-consistency. 

**Write path:**  are always sent to all replicas, regardless of consistency level. The consistency level simply controls how many responses the coordinator waits for before responding to the client.\
**Read path:**  the coordinator generally only issues read commands to enough replicas to satisfy the consistency level.\
Failure detection - based on a gossip protocol
### Transactions
Isolation - with compare and set (CAS) 
### Durability
Commitlogs are an append only log of all mutations local to a Cassandra node.\
Any data written to Cassandra will first be written to a commit log before being written to a memtable. This provides durability in the case of unexpected shutdowns.\
Memtables are in-memory structures where Cassandra buffers writes.\
In general, there is one active memtable per table.\
Memtables may be stored entirely on-heap or partially off-heap, depending on the configuration.

SSTables are the immutable data files that Cassandra uses for persisting data on disk.\
SSTables are flushed to disk from Memtables or are streamed from other nodes.

![partitioning](https://user-images.githubusercontent.com/58383975/97005007-81880080-1546-11eb-918b-f5836f3267c3.png)

### CAP
Cassandra chooses Availability and Partition Tolerance from the CAP. 
 
# CouchBase
Type: document store\
Couchbase Server is an open source, distributed data-platform.\
Language: N1QL (sql-like).\
Data document type: JSON\
Data can be retained either in memory only, or in both memory and storage.\
memory-first, async architecture\
Data can be replicated across the nodes of the cluster / data centers\
Couchbase is straightforward to deploy and manage. Features such as replication are built in with automatic sharding.\
Full text search \
Parallel query processing -  for execute complex, long-running queries that contain complex joins, set, aggregation, and grouping operations.\
High availability - all operations can be done while the system remains online.\
Use at: Linkedin, paypal,ebay\
***CouchBase Services:***
The services can be deployed and maintained independently of one another (mix & match)
 - Data: Supports the storing, setting, and retrieving of data-items, specified by key.
 - Query: Parses queries specified in the N1QL query-language, executes the queries, and returns results. The Query Service interacts with both the Data and Index services.
 - Index: Creates indexes, for use by the Query Service.
 - Search: Creates indexes specially purposed for Full Text Search. This supports language-aware searching; allowing users to search for, say, the word beauties, and additionally obtain results for beauty and beautiful.
 - Analytics: Supports join, set, aggregation, and grouping operations, which are expected to be large, long-running, and highly consumptive of memory and CPU resources.
 - Eventing: Supports javascript callbacks on mutations and deletions. 

 - Cluster Manager : control plane. runs on all the nodes of a cluster, maintaining node processes and coordinating cluster-wide operations like leader election . 
 - Master Services. rebalance, auto failover, sharding/ data partitioning
 - Bucket services
 
 ![couchbase servuces](https://user-images.githubusercontent.com/58383975/97005011-82209700-1546-11eb-85ad-37a0e1acb033.png)

### Caching and Persistence
Buckets: These store data persistently, as well as in memory.\
Ephemeral buckets: To be used whenever persistence is not required\
Memcached buckets: Designed to be used alongside other database platforms, such as ones employing relational database technology.\
By caching frequently-used data, Memcached buckets reduce the number of queries a database-server must perform.

![partitioning](https://user-images.githubusercontent.com/58383975/97005017-82b92d80-1546-11eb-9e77-e8d3faea711b.png)

### Durability
Clients writing to Couchbase Server can optionally specify durability requirements, which instruct Couchbase Server to update the specified document on multiple nodes in memory and/or disk locations across the cluster, before considering the write to be committed.  The greater the number of memory and/or disk locations specified in the requirements, the greater the level of durability achieved.

### CAP - CP
Couchbase Server acts like a CP system in its default configuration. This is because any access to a given key (read, write, update, delete) is always directed to the node that hosts that active data at that point in time\
Any write is also replicated within the cluster, but these replicas are primarily for the purpose of high availability and by default do not service any traffic until made active.
If a single node fails, the data on a node that failed will not accept writes until the node is failed over, although reads can be serviced from replicas if desired.
### Clustering
read and write directly to database nodes (by the cluster Manager).\
Couchbase Server has a flat topology with a single node type. All the nodes play the same role in a cluster, that is, all the nodes are equal and communicate to each other on demand.\
One node is configured with several parameters as a single-node cluster. The other nodes join the cluster and pull its configuration. After that, the cluster is operated by connecting to any of the nodes via Web UI or CLI. 
 
### Replication
Up to three replica buckets can be defined for every bucket.\
Each replica itself is also implemented as 1024 vBuckets.\
Typically, only active vBuckets are accessed for read and write operations, although vBuckets are able to support read requests.\
vBuckets receive a continuous stream of mutations from the active vBucket and are kept constantly up to date. 

![replication coucbase](https://user-images.githubusercontent.com/58383975/97005020-8351c400-1546-11eb-9178-968b8477d3d5.png)

 
### Partitioning
Automatic sharding\
shards calls vBackets.\
The cluster Manager keep vMap (Hash Table) that maps vBacket to node.\
key -> hash (0-1023 values) = vBacket -> node.\
Couchbase partitions data into 1,024 virtual buckets, or vBuckets, and assigns them to nodes.\
Like MongoDB chunks, vBuckets store all data within a specific range. However, Couchbase Server assigns all 1,024 virtual buckets when it is started, and it will not reassign vBuckets unless an administrator initiates the rebalancing process.

Couchbase Server clients maintain a cluster map that maps vBuckets to nodes. As a result, there is no need for routers or config servers. Clients communicate directly with nodes.

![partitioning](https://user-images.githubusercontent.com/58383975/97005023-83ea5a80-1546-11eb-8b55-1b37d7f42e72.png)

***Failure detection***
Failover can be performed manually or automatically using the built-in automatic failover process. Auto failover acts after a preset time, when a node in the cluster becomes unavailable. 
### Transactions
Insertion, mutation, and removal of multiple documents can be staged inside a transaction.\
Until the transaction is committed, these changes will not be visible to other transactions, or any other part of the Couchbase Data Platform. (dirty reads)
 
***Full text search***
words/documents. 
 
 
 
 
 


### notes - 
 **Availability -** the percentage of time that an asset is operating, compared to its total scheduled operation time. Alternatively, availability can be defined as the duration of time that a plant or particular equipment is able to perform its intended tasks.\
**Reliability -** Reliability quantifies the likelihood of equipment to operate as intended without disruptions or downtime. In other words, reliability can be seen as the probability of success and the dependability of an asset to continuously be operational, without failures, for a period of time.\
**Durability -** Durability  refers to long-term data protection, i.e. the stored data does not suffer from bit rot, degradation or other corruption. Rather than focusing on hardware redundancy, it is concerned with data redundancy so that data is never lost or compromised.\
**Distributed Hashing -** server = hash (key) modulo (number of servers)
Consistent Hashing : Distribution of data across a set of nodes in such a way that minimizes the re-mapping/ reorganization of data when nodes are added or removed





