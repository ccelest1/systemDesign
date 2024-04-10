# ACID V BASE: Comparison of Database Transaction Models

## Intro
- Consider project requirements when deciding on the right DBMS. A recommended first step is to choose a database transaction model
    - DB Transaction Model: set of rules that determine how a db organizes, stores, manipulates data

## What are the differences
- CAP [ Consistency, Availability, Partitioning ] Theorem states that it's impossible to achieve both consistency, availability in a partition tolerant distributed system
    - A system that works in case of temporary communication breakdowns
- Fundamental difference between ACID and BASE DB models is how they handle the above limitation
    - ACID provides a consistent system, BASE provides a system that is highly available

## ACID Model [ Atomic, Consistent, Isolated, Durable ]
-  Ensures that a performed transaction is consistent
    - Good Fit for Businesses
        - Online transaction processing (financial institutions)
        - Online analytical processing (data warehousing)
    - These orgs req db that can handle several small simultaneous transactions req 0 tolerance for invalid states
- Summary
    - A (Atomic): Either a transaction is executed or process halts and db reverts back to state prior to start of transaction, all data in db is valid
    - C (Consistent): Processed transaction doesn't endanger db integrity i.e foreign (one to one, many to one, many to many) and other constraints are enforced
    - I (Isolated): Transactions don't compromise the integrity of other transactions by interacting with them while in progress i.e no conflicting transactions
    - D (Durable): Data related to transaction persists in case of network/power outages or if transaction fails, there will be no impact to manipulated data

### ACID Use Case Example
- Financial institutions exclusively use this transaction model
    - With money transfers depending on the atomic nature of ACID i.e transactions or all or nothing ( retains db validity )
- Interrupted transactions that are not immediately removed from db can creates issues as money can be debited from one account and not credited to another

### ACID Compliant DBs
- In order to make db is acid compliant, choose rdbms like MySQL, PostgreSQL, Oracle, SQLite, Microsoft SQL Server
- NoSQL DBMSs like Apache's CouchDB, IBM's Db2
    - posses a certain degree of ACID compliance

- Philosophy of NoSQL approach to db management is in contrast with strict ACID rules
    - Not a rec for those req strict environments

## BASE Model
- NoSQL dbs provide flexible, fluid method of data manipulation -> new db model was designed, reflecting said properties

- BASE [
    B: Basically Available,
    S: Soft State,
    E: Eventually Consistent
]
    - B: As opposed to immediate consistency, BASE NoSQL dbs ensure availability by spreading, replicating across db cluster nodes
    - S: Data values change over time, BASE models delegates responsibility of consistency to developers
    - E: BASE transaction models may eventually achieve it, and until then data reads are feasible

### Base Use Case
- Marketing, customer service companies who deal w/ sentiment analysis prefer BASE elasticity when conducting social network research
    - Social network feeds are not well structured, contain huge data amounts that a BASE model db can easily store

### BASE Compliant DBs
- NoSQL dbs conform to BASE
    - MongoDB, Cassandra, Redis, Amazon DynamoDB, Couchbase

## Which One?
- Given their highly structured nature, ACID-compliant dbs are optimal for devs who req consistency, predictability, reliability

- Those who are req to heavily consider growth should choose BASE as it enables easier scaling, provides optimal flexibility
    - Req devs who know how to deal with model limitations
