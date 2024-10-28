https://webapi.teamviewer.com/api/v1/docs/index#/

# TeamViewer REST API Collector

This collector template allows you to collect logs from TeamViewer. 
Documentation for the REST API can be found here:

[TeamViewer API Reference](https://webapi.teamviewer.com/api/v1/docs/index#/)

## Pre-requisites
You will need to create an API key, aka script token to authenticate to the API.

TeamViewer have an article [here](https://www.teamviewer.com/en-au/global/support/knowledge-base/teamviewer-classic/for-developers/use-the-teamviewer-api/) that guides you through the steps of creating one.

## Configuring
### Placeholders
You must replace the following placeholders in the configuration:
* `API Key` - Your API Key aka Script token

### Body Params
* `Events` List - Make sure you customize the Events parameter to your preferences, according to the scope of events you want to be ingesting. See "Filtering the Events" below for more details.

Ensure you schedule this job and provide a relevant time range. The recommendation is to use the following:

Earliest: `-10m@m`
Latest: `-5m@m`

### Event Breaker

Import the provided event breaker (TeamViewer-AuditEvents).

# Filtering the events
## Event Logging
The TeamViewer EventLogging API provides a couple of days to specify exactly what you want to ingest. Events, and EventTypes.
Refer to the [documentation](https://webapi.teamviewer.com/api/v1/docs/index#!/EventLogging/EventLogging_Post) on EventLogging for detailed information and examples.

## Author
Daniel Harvey - [GitHub](https://github.com/snags141)