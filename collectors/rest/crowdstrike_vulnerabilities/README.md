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

## Importing and Configuring each REST Collector

From the top nav of a Cribl Stream instance or Group, select **Data > Sources**, then select **Collectors > REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST > New Collector** modal.

1. Click **Configure as JSON** to open the configuration editor.
2. Select **Import** from the top right and choose the collector.json file you downloaded from the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/crowdstrike_vulnerabilities).
3. Click **OK**.
4. A modal will open and prompt you for the two values you captured from your Falcon portal. Populate the form fields for `clientSecretParamValue_value` , and `client_id_value`. 
5. Click **Replace Values**.
6. Click **OK**.
7. Click **Save**.

## Testing
When running a collection, the time range is passed into the API query.  To configure time ranges, you'll use **Earliest** and **Latest** values.

- If you enter **Earliest** and **Latest values**, Cribl Stream will pass them into the API query.
- If you leave these fields blank, Cribl Stream will query the API for 24 hours before the job is scheduled to run.

Perform a Preview:

1. On the **Manage REST Collectors** page, click **Run** beside your new Collector.
2. In the resulting **Run Collector** modal, select **Mode > Preview**.
3. Configure a **Time range**, if desired.
4. Click **Run** to retrieve Preview results.

Automate your collection by clicking the **Schedule** button next to your new collector.
   
## Author
Jim Apger - japger@cribl.io
