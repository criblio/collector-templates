# Okta REST Collector

This collector template allows you to collect the Security Audit Logs from your Okta instance.

## Configuring

You must replace two placeholders in the configuration:

`<instance>` in the Collect URL must be replaced with our Okta tenant name. e.g. `goats.okta.com`

`<key>` in the Collect Request Headers must be replaced with the correct authorization token obtained from your tenant that has the correct permissions to read the security logs.

Auth key will be need to prefix the value with the SSWS in the format `SSWS <key>` as per [Okta API Docs](https://developer.okta.com/docs/guides/create-an-api-token/main/)

Ensure you schedule this job and provide a relevant time range. The recommendation is to use the following:

Earliest: `-10m@m`
Latest: `-5m@m`

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `Okta-API` event breaker rule set.

## Author
Brendan Dalpe - bdalpe@cribl.io
