# Goal
The goal of this project is to create an open, crowdsourced, distributed real-time geo database.

In simpler words, it is to provide two functions:
1. Realtime POIs like police traps or accidents, like existing centralized apps: [Yanosik](https://my.mixtape.moe/sylxdk.png), [Waze](http://imgur.com/oVg5fQS)
2. Near-realtime traffic speed, like [Google Maps Traffic layer](https://www.google.pl/maps/@52.2432257,21.026693,12.5z/data=!5m1!1e1)

Users would run an app on their devices, sharing POIs and (opt-in) anonymized location. The app would also display warnings about nearby POIs. 

# Requirements
Technically, the protocol needs to be p2p because:
* hosting such a large, rapidly-changing database would be prohibitively expensive
* an expensive database would require an organization or a business model to back it up
* organizations and businesses can't engage in illegal activities, and sharing police location is not legal everywhere

Because of limited bandwidth and processing power of mobile devices, some help from volountarly-ran nodes may be required, such as:
* seeding historical data
* calculating aggregate data (such as processing raw speed data and snapping it to nearest roads)
* providing HTTP gateways to low bandwidth devices
* feeding data to the network from centralized services

It must be possible to run such stationary nodes anonymously, without exposing one to potential lawsuits.

It must be possible to run such stationary nodes for a small region such as city or county. 

Because of the size of the data, both mobile devices and stationary nodes need to be able to access it in small chunks.

# Implementation
The p2p part of this project could be implemented with [Twister](https://twister.net.co/). Twister is a p2p micro-blogging application utilizing blockchain for username registrations, DHT for real-time updates and Torrent-like swarms for history retention.

Twister meets the goals of this project because:
* It's p2p
* Its hashtags have all the desired properties for real time updates:
 * They are real time
 * They are archived in swarms
 * A large number of tags may be created without impacting network performance
 * The messages are signed by an identity that's moderatively expensive to create *en masse*, creating means to prevent spam and bad behavior.
 
Hashtag based on approximate location could be used, #oy_522_210 would be used for point 52.2432257N,21.026693E and all other points within the range (52.2N..52.29999N, 21E.....21.099999E).

Mobile clients could follow hashtags where their current location belongs, as well as nearby locations.

Exchanged data could be human-readable to respect the nature of Twister network.

A message like:

`Patrol geo:51.6778,6.4877 #oy_516_64`

could mean that a police patrol has been seen at the given location.  Timestamped replies to this message could confirm or deny that POI. Timestimped votes could be passed through a time-based window function to give greater value to more recent votes.


GUI-wise, this could be implemented as a standalone app or an OSMAnd plugin.

For processing speed data, code from [Open Traffic](https://opentraffic.io) could be reused. Specifically, their [Traffic Engine](https://github.com/opentraffic/traffic-engine) "OSM street extracts and GPS points go in. Anonymized OSM-linked speed samples come out. "
