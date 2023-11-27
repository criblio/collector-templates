# Abuse.ch Threat Fox REST Collector

This collector template allows you to collect IoC information from the Abuse.ch Threat Fox API.

## Configuring

The API doesn't require any authentication and therefore nothing needs to be replaced in the collector.
The schedule (5 minutes after the hour) in this collector template to collect IoC from the last day is disabled. The timeframe can be adjusted by altering the collectBody.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `abuse-ch` event breaker rule set.

## Author
Christoph Dittmann - cdittmann@cribl.io
