# Alienvault OTX REST Collector

This collector template allows you to collect IoC information from your Alienvault Open Threat Exchange (OTX) subscription.

## Configuring

You must replace the following placeholder in the configuration:

`<OTX API Key|Your OTX Key (sign up for a key at [otx.alienvault.com](https://otx.alienvault.com/#signup))>` in the Collect Request Headers must be replaced with your Alienvault API Key

The schedule (5 minutes after the hour) in this collector template, to collect updates for IoC you are subscribed to, is disabled.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `alienvault` event breaker rule set.

## Author
Christoph Dittmann - cdittmann@cribl.io
