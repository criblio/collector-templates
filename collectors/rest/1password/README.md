# OnePassword REST Collector

This collector template allows you to collect threat details from the One Password API.

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

The collector is scheduled to run 15 minutes and to retrieve The Previous 15 Minutes, Currently there is a cursor functionality available via One Password but this has not been implemented here, This may be updated as additional testing occurs.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `onepassword` event breaker rule set.

## Authors
Matt Robinson -- mrobinson@cribl.io
Michael Buehler -- mbuehler@cribl.io

