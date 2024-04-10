# [System Design](https://www.youtube.com/watch?v=UzLMhqg3_Wc)

## ABCDS
- Ask good questions
    - What features are to be added?
        - work with interviewers to understand what feature set should include
    - How much should system be scaled?
        - req data to be stored in database, latencies expected in system
- Don't use buzzwords
    - Make sure there is indepth knowledge with all technologies discussed
- Clear and organized
    - Before minor details, define apis, write correct boxes, actors in system
- Drive discussions
    - Talk 80% of time, interviewer talks 20%

## Notes
 - Improve on ABCDS
    - Personal experience, improve via whiteboarding
    - Practice Sys Design through understanding solution implementation with current technologies

## Basics required for solving Sys Design Problems
- Discuss fts,define apis, availability, latency performance, scalability, durability, class diagram, security and privacy, cost effective
    - What happens when a db goes down?
    - caching and speed of performance, does the app scale with increased users and perform adequately
    - durability: data can be stored securely
    - class diagrams: how to design classes and applied OOP
    - security/privacy: design auth system
    - cost effective: is there an alt solution that will be more cost effective?

## Concepts to be aware of prior to interview
[Previous Doc on Sys Design](SdConcepts.md)
- Vertical v Horizontal scaling
    - Vertical means adding more CPU, memory, hardware to existing host, horizontal means adding more hosts (elimination of single failure point)
    - vert scaling can be expensive, limitation for memory/cpu at single host, horizontal has distribution challenges with less limitations

- CAP theorem [ Consistency, Availability, Partition (Network) ]
    * C - Reads have most recent writes, A - will get response back, P (tolerance) - between 2 nodes can be dropping network packets
    * Traditional: Choose C over A, DB is not always available but data consistency is preserved


- ACID v BASE (DB Transaction Models)

[ACID V BASE article](https://phoenixnap.com/kb/acid-vs-base)

- [ Atomicity, Consistency, Isolation, Durability ]
    - RDBMS are typically ACID compliant
        - A: every txn is all/nothing
        - C: Foreign keys, constraints are enforced
        - I: Concurrent transactions don't conflict
        - D: Data is persistent ( stored on disk, when machine restarts data continues its existence)

- [ Basically Available, Soft State, Eventually Consistent ]
    - NoSQL
    - Need to understand concept in order to understand which ACID principles are being sacrificed

- Partitioning/Sharding Data ( typically with noSQL )
    - trillions of records, can't be scored in 1 node of db -> how do you shard so that each node are responsible for a consistent part
    - consistent hashing
        - advs, disadvs

- Optimistic vs Pessimistic Locking
    - ( Optimistic Locking ) not required locking, but when ready to commit a transaction, check to see if no other transaction changed record that one will be working on
    - ( Pessimistic Locking ) acquire locks prior and then commit transactions
        - understand in which scenarios which are better suited

- Strong v Eventual Consistency
    - Strong: latest reads always see latest writes (ACID, i.e data consistency immediately )
    - Eventual: latest reads will eventually see latest writes ( BASE i.e E being eventual consistency) -> higher availability as it relates to CAP theorem with tradeoff with consistency

- Types of NoSQL
    - Key Value
        * Simplest where you have a key, value storing it in dict format in db
    - Wide Column
        * row can have many varying formats, columns and several columns
    - Document Based
        * if you have a semi structured db or if one has xml, json data
    - Graph Based
        * entities, edges/relationships between entities
- Caching
    - If there is data that is accessed more frequently, can store data in cache for easy access
    - Two types
        * Every node performs caching so cached data is not shared btwn nodes
        * Cached data is shared between varying nodes
    - (1) Cache can't be the source of truth, (2) Cache data has to be small as it tends to keep all data in memory, (3) Consider eviction policies around cache

- Data Center/Rocks/Hosts
    - Data center architecture or how data centers are arranged
    -  what is the latency regarding communication across racks
    - worst case: rack or entire data center goes down

- CPU/Memory/Hard Drive/Network Bandwidth
    - Limited resources that dictate how sys needs to be designed
    - how do you work around limits, improve throughput latencies, scale system along

- Random v Sequential read/write on disk
    - read,write on disk is usually slow but sequential
    - sequential reads and writes > random r+w, as random is slower than sequential

- http [ HyperText Transfer Protocol ] v http2 v Websockets
    - HTTP = request-reply architecture between client and server that entire web runs on
    - HTTP2 improves on deficiencies, can perform multiple requests over a single connection
    - websocket, fully bidirectional connect communication btwn client, server

- tcp/ip model
    - 4 layers of tcp/ip model

- ipv4 vs ipv6
    - ipv4 has 32 bit addresses, v6 has 128 bit addresses
        - as we are running of v4, world is migrating to v6
    - good to know how ip routing operates

- tcp v udp
    - tcp is connection oriented reliable, udp is unreliable connection
        - better to use udp with video streaming services, tcp with sending documents

- DNS Lookup
    - Domain name server lookup, allows for translation of facebook.com to an ip address

- https and tls
    - TLS = transport layer security used to secure communication between client, server in terms of privacy and data integrity
        - when used w/ HTTP it gets converted to that

- public key infrastructure and certificate authority
    - Used to manage public key, digital certificates, certificate authority -> trusted entity
    - Allows for auth of public key being from correct party

- symmetric v asymmetric key
    - asymmetric encryption is computationally more expensive -> used to send a small qt of data
        * Public private key encryption
    - v symmetric, an example is AES

- load balancer -> l4 v l7
    - Sit in front of service and delegate requests to one of nodes behind service
        - Delegation could be based on round-robin basis or load average, Lbs can operate at either l4 or l7 osi model levels
    - L4: Considers both client, destination IP addresses and port numbers to perform routing
    - L7: HTTP level uses http url to perform routing with most lbs operating at l7

- CDNS and Edge
    - CDN Example: Netflix will put movies, media series in cdn close to viewer -> allowing movie to be streamed from closest location instead of all the way from the data center
        - aids in performance, latency for end user
    - Edge = similar concept where processing is performed close to end user
        - dedicated network from edge to data center, request can be routed through dedicated network instead of general internet

- Bloom filters and count-min sketch
    - bloom filters allows for decisions to determine if an element is a member of set or not
        - can have false positives, never false negatives (if design can tolerate false positives then use bloom filter as it is space efficient)
    - count-min used to count frequency of events
        - if you have a million events, want to keep track of top events then consider using count made sketch
        - provides a close enough to the actual with the fraction of space, including an error rate

- Paxos (Consensus over distributed hosts)
    - Perform leader election among a distributed host

- [Design Patterns and Object Oriented Design](DesignPatterns.md)
    - DPs: Factory, Singleton
    - OOD: Abstractions, Inheritance
    * leader election

- Virtual machines and containers
    - VMs provide ability to provide an OS on a shared resource such that you are the exclusive owner of hardware, while in reality this hardware is shared between varying isolated OSs
    - Containers allow one to run apps, dependencies in an isolated
        - run heavily in production environments

- Pub-Sub / Queue
    - Pub sends message to queue that a sub is subbed to
    - customer facing requests should not be directly exposed to a pub sub system
- MapReduce
    - Perform distributed and parallel processing of big data
    - Filtering and sorting data with map, reduce summarizes

- Multi threading, concurrency, locks, synchronization, CAS
    - Critical for multi threading process, various languages have varying implementations

## TOOLS
- Cassandra
    - wide column highly scalable db, used for a variety of use cases such as simple key value storage or storing time series data or more traditional rows with several columns
        - can provide both eventual, strong consistency
        - uses consistent hashing to shard data
        - uses gossiping to keep all nodes informed about cluster

- MongoDB/Couchbase
    - (MDB) if you have JSON like structure and if you want it to persist, mongo works
        - provides ACID properties at a document level, scale effectively

- MySQL
    - Traditional use case with several tables, relationships with tables and full ACID properties
    - Master-slave architecture

- Memcached, Redis
    - Distributed cache, hold data in memory using simple fast key-value storage
    - Redis also performs key value storage, can be set up as a cluster and provides more availability, data application

- ZooKeeper
    - Centralized config management tool used for leader election, distributed locking
    - Scales well for reads, writes
    - Keeps all in memory, so data storage is heavily limited

- Kafka
    - Fault tolerant highly available queue using pub/sub or streaming app
    - Depending on use case, can deliver messages exactly once, keeps messages ordered in partitions

- NGINX, HAProxy
    - Load balancers that are efficient, can manage several connections from a client in a single instance

- Solr, Elastic Search
    - Search platform, both are highly available, scalable and tolerant
    - provide full text search

- Blobstore (AWS S3)
    - have big pictures, files images then store it in cloud
    - S3 is provided as part of AWS platform

- Docker (software platform for containers in order to develop and run distributed applications)
    - containers can run on laptop or at data center or cloud
    * Kubernetes, Mesos
        - tools to manage, coordinate containers

- Hadoop / Spark
    - under the hood has mapReduce occurring, processing occurring on a large panel of data
    - Spark is faster version, Hadoop relies on HDFS for processing
    * HDFS
        - Java based file system which is distributed, fault tolerant
