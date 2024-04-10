# [SPOTIFY SYSTEM DESIGN](https://www.youtube.com/watch?v=_K-eupuDVEc)

## HOW WOULD YOU DESIGN SPOTIFY
ws = webserver
- Spotify is one of the major music listening
    - Several aspects for design
        * Spotify has songs ('music'), playlists, users, artists, podcasts
- Use Cases
    * Finding and playing music
    * Browse for music -> play song and said songs comes back through device

> - __Clarifying Questions__
    >   - Constrain problem to make it solvable in hour
    >   - Share what is known regarding potential system with interviewer
    >   - if not extremely/100% filled in, interviewer can supplement information

- Numbers
    - **_Calculate Amount of Data Dealing With_**
        * Users: 1 billion
        * Songs: 100 million
        * mp3 audio: 5MB
            - Total audio: 50TB = 0.5PB
            - multiple copies for preservation is 3x replication so 1.5pb of data
            - 100B per song metadata -> 100gb song metadata
            - 1kb per user >> 1TB

> - __Metrics__
    >   - Specify some key metrics to aid in high level decision-making
    >   - In this case we were able to properly
    >   - calculating traffic per day and per second would've helped

- Components
    * Lay out basic sys design components
    * Spotify App
        - phone talks to several spotify web servers
        - app talks through load balancer to web servers
        - web servers read/writes to db

> - __High Level__
>   - Practice and ensure you are carrying out dual bandwith comms, okay to have simple comps at early stage -> get more complex

- Outline DB Design
    * two dbs are split, song audio db + metadata db (users, songs, artists, ...)
    * ws are talking to two dbs: metadata db (aws rds), audio db (aws s3)
        * s3 as a blob store lends itself very well to strong streaming and scales with further addition performing in a linear fashion
        * metadata with users, songs, artists in order to perform queries - remember where i left on on a song will go back and forth with relational database (mysql)
    - (metadata) with songs you have a song_id, song_url, artist, genre, link to album_cover + audio_link
        * queries, updates, etc
    - (audio db) song mp3
        * separated due to access patterns, data size, queries that req to occur

> - __DB__
>   - after laying out comps, design db
>   - split dbs appropriate for various data types
>   - proactively explain decision-making

- Finding and playing music use case
    * spotify needs to do a get/find request
    * search for music with specified genre with clicking or finding
        - return list of songs with associated metadata and audio file
        - once found song, click on song -> translate request from webserver to play specified song -> webserver based on song info has to use mp3 link to fetch actual song from db
        - chunk by chunk requires websocket connection (don't start streaming until data is in memory)
    * start playing -> web server gets request to start playing song -> gets info to s3 -> read back (5mb) -> stream to app

- Bottleneck
    * going to be indie artists that aren't that popular
    * highly anticipated request for new project -> easily overload s3 resulting in high bandwith
    - Can use a CDN in order to reduce constant load of service
        * CDN (song audio cache) which is typically close to users
    - spotfiy ws need a tracker for most highly requested songs and adapt to highly request datat to CDN
    - CDN loads request for application, before talking to webserver check with CDN or webserver sends a redirect to CDN to get song (AWS CloudFront)
        - can also create local cache, share cache among servers
        - if edge caching, offloading ws
        - store song, locally user-dependent
        * i.e. there is multi user caching

> - Drill Down
>   - talk through uses cases for design checks, id bottlenecks
>   - id existing technologies like cdns, caching
>   - discuss how solutions like caching can apply at various levels

- Load Balancer Considerations
    * Ensure that ws provide good service to end users
    * ensure that there is = network bandwith i.e io/network bound (don't have lb send it more traffic)
    * one metric for lb is memory, traffic, requests outstanding, current streaming

    > - not a one size fits all, consider several metrics when applying tech like LB
    > - be open about expertise limits

- Other
    * Replication allows for data availability in potential outages
        - place replicas closer to where users are located
        - european punk rock streamed close to eu
            * replicas of song are closer to those locales
            * data placement, replication strategies

> Interview Conclusion
    > - __Wrap up answer by outlining how it meets reqs__
    > - __Interview End = think big, introduce new dimension__
