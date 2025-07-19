# Okta Apps REST Collector

This collector template allows you to collect okta applications and their affiliated users and groups.  

## How it works
The process runs a discovery request against the okta applications endopint, which returns app details and href links to lists of users & groups.  The collection process is run on these href links.  The parent app data is available to the events via the cribl __collectible field.

## Configuring

You must replace two placeholders in the configuration:

`{{OKTA DOMAIN}}` in the Collect URL must be replaced with our Okta tenant name. e.g. `goats.okta.com`

`{{API TOKEN}}` in the Collect Request Headers must be replaced with the correct authorization token obtained from your tenant that has the correct permissions to read the security logs.



### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `Okta-API` event breaker rule set.



