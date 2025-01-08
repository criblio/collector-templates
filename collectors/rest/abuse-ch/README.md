# Abuse.ch Threat Fox REST Collector

This collector template allows you to collect IoC information from the Abuse.ch Threat Fox API.

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
10) If there are fields for you to fill out, you will be prompted here. When done, hit Replace variables.
12) Commit and Deploy

## Configuring

The API doesn't require any authentication and therefore nothing needs to be replaced in the collector.
The schedule (5 minutes after the hour) in this collector template to collect IoC from the last day is disabled. The timeframe can be adjusted by altering the collectBody.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `abuse-ch` event breaker rule set.

## Author
Christoph Dittmann - cdittmann@cribl.io
