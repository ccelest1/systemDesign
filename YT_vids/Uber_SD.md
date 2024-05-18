# [Uber SD](https://www.youtube.com/watch?v=wL-Gx5XE9XE)
## Write out system functional, non-functional reqs
* clarify question scope w/ interviewer (good at product first companies by diving into strong minutia/details)
    - else -> don't spend too long listing every req if not interviewing at product first company
### Functional Reqs
- Riders: book rides from a -> b (now v later)
- Drivers: drive riders from a -> b
    * rider actions - book ride, use gps + from provided point:
        1. set custom time, change ride mid way, cancel ride + before/mid
        2. different car types, number of people be picked up can be different
    * diver actions - provide ride:
        - mark self as driver, 'on hold'
            - 'I'm heading home' mode in order to avoid drivers from being penalized
            - consider 'automatic accept rides until T+2 hours/5PM'
        - start reception of 'bids'
            - receive pick up location, time 'now'
            - how long/far is ride planned to be, extra req metadata
            - prospective rider's rating
    * shared requirements:
        - rider + driver ratings
        - pickup/dropoff points

### Non-Functional
- Obj: Keep drivers busy
* __focus on rider <> driver matching problem__
- Matching Algo + UX
- Understand how process goes from right to left of above objective (life of a ride)

## How a ride progresses
1. Rider request
2. System starts looking for driver
3. FIND_A_DRIVER
    - IF driver who accepted ride is found, invoke `PICKUP()`
4. If instant book available -> (a) pick driver nearby -> (b) send ride request -> (c) if no explicit rejection, wait 4 sec then invoke `FIND_A_DRIVER()`
5. If instant book not available -> (a) pick driver nearby -> (b) send ride request -> (c) if no conformation, wait 7 sec then invoke `FIND_A_DRIVER()`

- PICKUP
    - location shared in real time, tweaked to allow for traffic lights + other traffic rules
- PICKUP_MODE()
    * Can cancel from either side, for potential fee
    * Once ride is confirmed go to IN_RIDE mode
        * confirmed includes gps, manual driver confirmation, code

## High Level Design
-

## Metrics + Reflection
-
