# [SYSTEM DESIGN: Design TinyURl Service](https://www.youtube.com/watch?v=fMZMm_0ZhK4)

## Design Requirements
> Need to design a service where a user provides a long URL -> return a short url and when the short URL is provided -> return long url
- Presents design challenges that need to be handled at scale
- Using a map is neither scalable or durable in nature

## Design Process
> When designing a service, it is good to prioritize:
 - API
    - How a user will interact with service
        - createTiny(longUrl) -> tinyUrl
        - getLong(tinyUrl) -> longUrl

- Application Layer
    - How do you generate a tinyUrl that is unique? (7-8 chars long)

    ![App Layer](../../Images/TinyUrlAppLayer.png)
        - Customer/client talks to service either through REST, HTTP, other available open source software
        - Request via REST then interacts with Load Balancer (front end for service), software or software + fiber combo at minimum performs delegation of requests to one of worker threads
            * LB can also perform other processes depending on config
        - worker threads/host take the url and generate the tinyUrl and store them both in persistence layer -> (when there is a get request) -> get longerUrl for shorter Url and return to client
        - Caching Layer (memcache or redis) can be implemented

![DB Techs](../../Images/dbApproaches.png)
## Approaches
- (1st method: Generate Random Tiny Url)
    - How do u generate a tinyurl that is as unique as possible
        - chars that we can have in url include:
            - a-z (26), A-Z (26), 0-9 (10) so in total is 35 trillion combos (43 bits)
            - if we are doing millions of requests daily, this 35 T will be exhausted in 40 days
        - Techniques to generate most unique seven char urls
            - DB: Table Schema
                * key as the tinyUrl, value as the long
            - 1st Technique
                - Generate the tinyUrl -> check db
                - Worker host gets a request from a user who wants to convert a long url to short url -> then generate a random tinyUrl
                - Then perform a get on the tinyUrl -> check to see if what is generated exists in db or not
                    - if tinyurl doesnt exist in db, put tinyurl and longurl in db
                    - ^ the problem that arises: for another longurl the same tinyurl can be potentially generated which can cause an irregularity -> a stable connection sys doesn't deploy this technique

            - 2nd Technique [ works if u have db support]
                - Put if absent into db and req support from db
                    - put tinyurl, longurl into db if there is no key whose value is equal to this tinyurl
                    - for relational dbs like mySQL, Oracle that support ACID, it is trivial and for noSQL it is harder
                    - noSQL scale better and some sqls like oracle are expensive

            - 3rd Technique
                - worker gens a random tinyUrl then he puts that generated url into db then check if longer url attached to tiny is same that is inputted, else generate another tinyurl until it is not currently attached to a longurl

- (2nd method: MD5 Approach) [ Saves on space as compared to the first method ]
    - calculate md5 of longer url -> then take the first 43 bits of md5 -> then generate tinyUrl
    - md5 = hashing function that generates 128 bit long hash and take 43
        - probability of collision:
            * if you take more bits of 128 then there will be less probability of collision, string will get longer
            * if you take less, higher probability of collision but tinyUrl gets shorter
    - So after gen 128 bits, take 43 bits to gen tinyUrl -> apply check db technique
        - how does 43 bits convert to a tinyUrl: 43 bits are just 0s and 1s
        - convert into decimals by raising 2s by 0s and 1s -> convert num to base62 -> map numbers to characters using the numerical order of the alphabet

- Counter Based Approach (different methods)
    * [1st Technique = Single Host]
        - Single host is responsible for maintaining counter for db or a zookeeper instance
        - When worker hosts obtain request with tiny Url they talk to counter host i.e the maintainer -> return unique number  + increment counter internally -> repeats for each worker request
            - Problems with this approach is single point of failure and bottleneck
            - if this host goes down, takes time for another host to assume responsibilities
            - if number of requests is high, counter host may not be able to handle demand
    * [2nd Technique = All Host]
        - Every host tries to maintain the current counter system
        - 64 worker host, assign unique 6 bits to every host and take current timestamp -> then add 5 bits of incremental or random value total 43
        - high request = high collision
        - reqs more bits and increase last few digits that are random/incremental
    * [3rd Tech = Range Based Approach ]
        - Use as many combos as possible and scales well, worry about first billion of combos of million ranges each
        - Maintained in zookeeper, request from worker asks for an unused range
            - each worker increments in a particular non conflicting range, guarantees no collisions
            - can add new workers that ask for new ranges
        - if done using all ranges, then create more ranges by increasing upper limit to trillions
            - problem with approach as generate url is in sequential manner
                - can take number and add random bits for url generation

- Persistence Layer
    - where and how short and long urls are stored

## Summary
! Important to cache tinyurl ! (redis, memcache)
![Summary](../../Images/summary.png)
