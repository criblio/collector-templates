# CrowdStrike Vulnerability Management API REST Collector

This collector template allows you to collect logs from Crowdstrike Alerts.

Refer to Crowdstrike's API documentation [here](https://falcon.us-2.crowdstrike.com/documentation/page/ab572b16/vulnerability-management-apis)

You will need the below values from your Falcon portal to complete the integration:

| Item | Value |
| ----------- | ----------- |
| Client ID | 32-character string |
| Client Secret | 40-character string |

## Downloading Configuration Files

Download the required event breaker and REST Collector JSON configuration files from [this Cribl repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/crowdstrike_vulnerabilities).  You will import these in the below steps. 

## Import the Event Breaker

The Event Breaker strips headers from events, leaving only the records of interest and capturing timestamps properly.  You will have a single event for each unique record.  The event breaker is configured to set the timestamp equal to the updated_timestamp field but you can modify it to use the created_timestamp field if needed.

1. Navigate to **Manage > Processing > Knowledge > Event Breaker Rules**.
2. Click **Add Ruleset**.
3. Click **Manage as JSON** at lower left.
4. Click **Import** at top right.
5. Select the breaker.json file you downloaded from [here](https://github.com/criblio/collector-templates/tree/main/collectors/rest/crowdstrike_vulnerabilities).
6. Click **OK**.


## Author
Jim Apger - japger@cribl.io
