# Google Workspace Reports Api  REST Collector

This collector template allows you to collect threat details from the Google Workspace Reports API.

# Installation

1) Copy the contents of breaker.json
2) Navigate to Event Breaker Rules under Processing -> Knowledge
3) Create a new breaker ruleset
4) Edit as JSON and paste the previously copied JSON into place; save
5) Copy the contents of collector.json
6) Navigate to Data -> Sources -> REST Collectors
7) Add Collector
8) Configure as JSON tab (at the top of the window)
9) Paste the JSON from collector into the screen and save
10) Commit and Deploy

## Configuring

The collector is scheduled to run every 15 minutes and to retrieve either the past 15 minutes or else the  'time now-15 minutes' value if no earliet and latest are provided.

Make sure to update the Subject with the Content desired in the Subject field, as well as the service account credentials field as well. 
 

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `google_sdk` event breaker rule set.

## Authors
Scott Cutler - scott.cutler@rula.com
Michael Buehler - mbuehler@cribl.io -- Just Cleanup for Templates. 

