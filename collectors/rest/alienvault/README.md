# Alienvault OTX REST Collector

This collector template allows you to collect IoC information from your Alienvault Open Threat Exchange (OTX) subscription.

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

You must have the following info at hand to complete the set-up above:

OTX API Key: Your OTX Key (sign up for a key at [otx.alienvault.com](https://otx.alienvault.com/#signup))> in the Collect Request Headers must be replaced with your Alienvault API Key

The schedule (5 minutes after the hour) in this collector template, to collect updates for IoC you are subscribed to, is disabled.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `alienvault` event breaker rule set.

## Author
Christoph Dittmann - cdittmann@cribl.io
