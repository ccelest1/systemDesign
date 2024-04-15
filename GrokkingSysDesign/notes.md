# Sys Design Interviews - Step by Step
- __TO BE SUCCESSFUL__: preparation + commit to organization via these steps in order to be on track + cover various system aspects

## 1. Requirements Clarification
- __MUST__: Ask questions about exact scope of problem being solved
    * design questions are open-ended and don't have one exact correct answer -> clarifying ambiguities is critical
    * candidates who spend large amounts of time to define system end goals have better chance at interview success
        - __EXAMPLE__: If designing a Twitter like service:
            * Qs: Will users of service post tweets + follow others, user timeline?, photos + videos, focusing on be and/or fe?, tweet search, displaying hot trending topics, push notifications?
## 2. System Interface Definition
- Define expected APIs expected from system -> establish expected contract
    * Twitter service examples:
        - postTweet(user, data, tweet_location, ...), generateTimeline(), markFavoriteTweet()
## 3. Back of envelope Estimation
- Great idea to estimate scale of system that is going to be designed in order to aid in our focus of scaling, partitioning, load balancing, caching
    * expected scale?, required storage?, what is the expected network bandwith usage?
## 4. Defining Data Model
- Defining data model early clarifies how data is to flow among various system components -> guide towards data partitioning + management
    - Req to id various system entities, how they will interact with each other, various aspects of data management (storage, transportation, encryption)
        * Twitter entities:
            - user(id, name, email, dob, ...), tweet(), userFollowers(), favoriteTweets()
        - which db system should be used (nosql - cassamdara = great for partitioning/sharding, horizontal scaling v mysql) and what block storage for photos + videos
## 5. High Level Design
- draw block diagram w/ 5-6 boxes representing system comps -> id comps req to solve actual problem
    * @ Twitter, for high level: req several app servers for read/write reqs w/ LBs for traffic distributions -> assume we will have more read traffic v write, decide to have separate servers for handling said load
    * BE = require efficient db to store tweets, support large number of reads -> req distributed file storage sys for storing photos, videos
## 6. Detailed Design
- dig deep into 2 or 3 comps -> present various approaches, pros + cons, explain reference towards one approach vs other
    * Storing massive data amt -> how do we partition data to distribute to several dbs, should we store all user data on same db + what issues can result
    * how to handle hot users
    * as user timeline contains most recent (relevant) tweets, should data be stored to optimize for scanning last tweets?
    * how much, what layer should cache be introduced
    * what comps req for better load balancing?
## 7. ID and Resolve Bottlenecks
- discuss any possible bottlenecks and try to figure out approaches for mitigation
    * any single point of failure in sys -> what is being done for mitigation
    * do we have enough replicas in case of lost servers
    * are there enough copies of various services in case that a few failures result in system shutdown
    * how is service performance being monitored? are there alerts for critical comp failure, performance degradation?
