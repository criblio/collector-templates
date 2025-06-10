# SailPoint V3 Search REST Collector

The /v3/search endpoint in the SailPoint IdentityNow API allows you to query a variety of object types, but specifically for events, you can retrieve audit or system activity logs that track changes, actions, and processes within your Identity Security Cloud environment

[Sailpoint V3 Search API Documentation](https://developer.sailpoint.com/docs/api/v3/search-post)

## Installation

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

The collector is scheduled to run 15 minutes and to retrieve either the 15 minutes or else the  'state.latestTime' value retrieved from state tracking.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `sailpoint` event breaker rule set.

## Authors

Andrew Hendrix - ahendrix@criblservices.io
Stacy Simmons - ssimmons@cribl.io

