# Boomi REST Collector

This collector template allows you to collect logs from Boomi's api.

# Installation

1) Copy the contents of boomi_eb.json
2) Navigate to Event Breaker Rules under Processing -> Knowledge
3) Create a new breaker ruleset
4) Edit as JSON and paste the previously copied JSON into place; save
5) Copy the contents of boomi_rc.json
6) Navigate to Data -> Sources -> REST Collectors
7) Add Collector
8) Configure as JSON tab (at the top of the window)
9) Paste the JSON from collector into the screen and save
10) If there are fields for you to fill out, you will be prompted here. When done, hit Replace variables.
12) Commit and Deploy

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
