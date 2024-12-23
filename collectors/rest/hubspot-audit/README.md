# Hubspot Audit Collector

This collector template allows you to collect the account activity logs from Hubspot.

## Configuring

You must replace one placeholders in the configuration:

`<Your Private App Access Token>` in the collect request header authorization value must be replaced with your access token from Hubspot when you create a Private App

This collector utilizes the state tracking feature. 

One quirk to note: The OccurredAfter filter in the collect URL actually behaves as "occurred after or equal to". To mitigate this, we simply increment the timestamp by one millisecond to avoid duplicate collection.

### Event Breaker

Import the provided event breaker.

## Author
Cam Borgal - cborgal@cribl.io