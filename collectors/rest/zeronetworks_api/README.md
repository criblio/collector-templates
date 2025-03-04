# Zero Networks API REST Collector

This collector template allows you to collect threat details from the Abnormal Security Threats API.

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
10) The collector uses a bearer token auth header. Retrieve your auth token via the Abnormal portal and save it as a secret. It's named 'abnormal_token' in this configuration.
12) Commit and Deploy

## Configuring

The collector is scheduled to run every 10 minutes starting 5 minutes after the hour 5,15,25,35,45,55 etc, and to retrieve either the current time minus 10 minutes.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `abnormal_threats` event breaker rule set.

## Authors
Eugene Katz -- ekatz@cribl.io
Michael Buehler -- mbuehler@cribl.io

