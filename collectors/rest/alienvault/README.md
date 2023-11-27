# Abuse.ch Threat Fox REST Collector

This collector template allows you to collect IoC information from the Abuse.ch Threat Fox API.

## Configuring

You must replace the following placeholder in the configuration:

`<your-otx-api-key>` in the Collect Request Headers must be replaced with our Alienvault API Key

The schedule (5 minutes after the hour) in this collector template, to collect updates for IoC you are subscribed to, is disabled.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `alienvault` event breaker rule set.

## Author
Christoph Dittmann - cdittmann@cribl.io
