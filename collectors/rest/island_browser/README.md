# Island Browser REST Collector

This collector template allows you to collect threat details from the Island API.

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

Make sure to update the Api-Key Field with your APi Key:

	this needs to be generated in the Island Management Console under settings > API and put in single quote (eg ‘apikeyvalue’) 

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `island` event breaker rule set.

## Authors
Michael Buehler - mbuehler@cribl.io

