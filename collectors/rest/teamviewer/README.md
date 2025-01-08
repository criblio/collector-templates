https://webapi.teamviewer.com/api/v1/docs/index#/

# TeamViewer REST API Collector

This collector template allows you to collect logs from TeamViewer. 
Documentation for the REST API can be found here:

[TeamViewer API Reference](https://webapi.teamviewer.com/api/v1/docs/index#/)

## Pre-requisites
You will need to create an API key, aka script token to authenticate to the API.

TeamViewer have an article [here](https://www.teamviewer.com/en-au/global/support/knowledge-base/teamviewer-classic/for-developers/use-the-teamviewer-api/) that guides you through the steps of creating one.

## Installation and initial config

1) Copy the contents of breaker.json
2) Navigate to Event Breaker Rules under Processing -> Knowledge
3) Create a new breaker ruleset
4) Edit as JSON and paste the previously copied JSON into place; save
5) Copy the contents of collector.json
6) Navigate to Data -> Sources -> REST Collectors
7) Add Collector
8) Configure as JSON tab (at the top of the window)
9) Paste the JSON from collector into the screen and save
10) If there are fields for you to fill out, you will be prompted here. When done, hit Replace variables.
12) Commit and Deploy

### Body Params
* `Events` List - Make sure you customize the Events parameter to your preferences, according to the scope of events you want to be ingesting. See "Filtering the Events" below for more details.

Ensure you schedule this job and provide a relevant time range. The recommendation is to use the following:

Earliest: `-10m@m`
Latest: `-5m@m`

# Filtering the events
## Event Logging
The TeamViewer EventLogging API provides a couple of days to specify exactly what you want to ingest. Events, and EventTypes.
Refer to the [documentation](https://webapi.teamviewer.com/api/v1/docs/index#!/EventLogging/EventLogging_Post) on EventLogging for detailed information and examples.

## Author
Daniel Harvey - [GitHub](https://github.com/snags141)
