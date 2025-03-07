# GraphQL REST Collector

This collector template allows you to collect threat details from the GraphQL API.

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

The collector is scheduled to run every 5 minutes and to retrieve either the past 5 minutes and use the current state  'state.latestTime' value retrieved from state tracking.

Make sure you include your API Key into X-API-KEY 
update your collect URL for your endpoint i.e. https://api.<Customer URL>.com/api/v1/graphql2
You can update the Collect Body to reflect any changes in your GraphQL query needed


### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `Matts Please Just Give Me Everything Ruleset` event breaker rule set.

## Authors
Brian Sampsel - bsampsel@cribl.io
Matt Robinson -- mrobinson@cribl.io
Michael Buehler -- mbuehler@cribl.io

