## System Design Concepts
 [Video](https://www.youtube.com/watch?v=i53Gi_K3o7I)
 - Vertical Scaling (add more resources to same server)
    - Server is accepting requests and fulfilling them when sending back responses to users
    - Increase in users = scaling by upgrading RAM, CPU

- Horizontal Scaling (add more servers)
    - Better approach
    - Add more replicas to handle subsets of requests, more powerful with infinite capacity
    - Eliminates a single point of failure

- Load Balancer: help spread traffic across cluster of services to improve responsiveness, app/site/db availability
    - To make sure that on server doesn't get overloaded while others sit idle, involves a Load Balancer which is a Reverse-Proxy Server that routes incoming requests to the appropriate server (replica)
    - Use Round Robin algo that balances traffic by cycling pool of servers
    - Hash incoming request id and even out amount of traffic each server receives
    - If server is located all around world, use LB to route to nearest location

- CDN (Content Delivery Networks)
    - Best use case is for Static Content
    - CDNs dont run app logic
    - CDNS take files from Origin Server -> Copy files onto CDN servers either through Push/Pull Basis to be distributed throughout network

- Caching
    - CDNs are a caching technique
    - Create copies for data that can be retrieved faster
    - Cache data onto computer disk to save Network expenses
        - Reading disk can be extremely costly so Computers copy onto memory
        - Reading memory can be expensive so a subset can be stored into L1, L2, L3 Cpu Cache

- TCP/IP (Internet Protocol Suite) includes UDP
    - Used by computers to communicate over networks
    - Protocols of sending data over internet
    - Files are broken down as packets and packets are numbered to be rearranged to resemble original file
    - HTTP and WebSocket are built on TCP

- Domain Name System
    - Decentralized service that translates domain to IP address
    - DNS Registrar allows for creation of a DNSA (Address) Record that allows for the mapping of an IP address to the url

- HTTP (HyperText Transfer Protocol)
    - TCP is too low level so HTTP is preferred for websites, don't have to worry about indv data packets
    - Application Layer Protocol like HTTP that follows client, server model
        - Client initiates request that includes request header (metadata, who is sending, where its being sent to), request body (package content)
        - HTTP includes several API patterns (REST, gRPC, etc)

- REST
    - REST = standardized, stateless, consistent
    - Successful request = 200 code, bad request from client = 400, issue with server = 500

- GraphQL (introduced by fb in 2015)
    - make single request via query
    - Query allows for choosing exact resources
        - fetch multiple resources with single request
        - no overfetching, so don't request data that is unneeded
- GRPC
    - RPC Framework for server to server communication (normal GRPC)
    - GRPC Web
        - in web, growing quickly and has performance boost with protocol buffers vs JSON
        - Data is serialized into binary format and is more efficient in terms of storage
        - However, JSON is human readable

- Websocket
    - Chat Apps (message sent -> receive it immediately)
    - With HTTP, would have to use polling
    - WS supports bidirectional communication
    - Get message immediately -> device pushed
    - Send message immediately -> push to other person's device

- SQL
    - Relational DBs (MySQL, Postgres)
    - More efficient storage with B Trees, SQL queries
        - B Trees: Efficient storage, SQL (structured query language): Fast and powerful data retrieval

- ACID
    - Relational DB Management systems are usually ACID compliant
    - Atomicity: Every transaction is all or nothing
    - Consistency: Foreign keys and other constraints are always enforced
    - Isolation: Different concurrent transactions don't conflict with each other
    - Durability: Data is persistent (data stored on disc and when machine restarts data continues to exist)

- NoSQL
    - Consistency with SQL-Relation DBs make scaling difficult
    - NoSQL drop consistency, relations
        - Sharding: Not having to enforce consistency allows for breaking up of DB and scale horizontally on different machines
        - Shard Key: decide which portion of data to put on which machine, given table -> shard key = id of person
    - Types:
        - Key Value Stores, Document Stores, Graph DBs

- Replication
    - Create read only DB copies (Leader-Follower Replication)
        -   Every write is sent to leader which is sent to followers
        - Every read goes to leader or follower
        - Leader-Leader replication can cause inconsistent data

- CAP Theorem (weigh tradeoffs with replicated design)
    - Pick 2 of three
        - Consistency
            - Data Consistency
        - Availability
        - Partition (Network)

-  PAC Theorem
    * (More complete version of CAP Theorem)
    ![PAC](Images/PAC.png)

- Message Queues
    - Durable Storage, Replicated for Redundancy, Sharded for Scalability
    - If system was receiving too many messages/data than can be processed, MQ allows for data to be persisted prior to processing
    - Different parts of app can be decoupled
