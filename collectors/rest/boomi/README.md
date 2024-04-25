# Boomi REST Collector

This collector template allows you to collect logs from Boomi's api.

## Configuring
* The URI token stub, show as `uriToken`. This will be unique to your org. It will go on the end of https://api.boomi.com/api/rest/v1/
* The Boomi access token, `boomiToken` roughly equivalent to a username
* The Boomi access password, `boomiePassword`
* Optionally you can hard code the destination and pipeline in the config as well. Alternatively, use the routing table as normal
* Finally, enable and adjust the schedule if required

### Event Breaker
* Use the included EB


## Author
Jon Rust <jon@cribl.io>