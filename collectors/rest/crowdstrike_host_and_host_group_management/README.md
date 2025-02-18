# CrowdStrike Host And Host Group Management API REST Collector

This collector template allows you to collect logs from the Crowdstrike Host and Host Group Management API.

Refer to Crowdstrike's API documentation [here](https://falcon.us-2.crowdstrike.com/documentation/page/c0b16f1b/host-and-host-group-management-apis)

You will need the below values from your Falcon portal to complete the integration:

| Item | Value |
| ----------- | ----------- |
| Client ID | 32-character string |
| Client Secret | 40-character string |

## Downloading Configuration File

Download the REST Collector JSON configuration files from [this Cribl repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/crowdstrike_host_and_host_group_management).  You will import these in the below steps. 

The above config uses a Cribl built-in event breaker (Cribl - Do Not Break Ruleset).  There is no breaker.json file for this config.

## Importing and Configuring each REST Collector

From the top nav of a Cribl Stream instance or Group, select **Data > Sources**, then select **Collectors > REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST > New Collector** modal.

1. Click **Configure as JSON** to open the configuration editor.
2. Select **Import** from the top right and choose the collector.json file you downloaded from the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/crowdstrike_host_and_host_group_management).
3. Click **OK**.
4. A modal will open and prompt you for the two values you captured from your Falcon portal. Populate the form fields for `clientSecretParamValue_value` , and `client_id_value`. 
5. Click **Replace Values**.
6. Click **OK**.
7. Click **Save**.

## Testing
There is no time reference in this API query, earliest and latest values from the scheduler are not relevant.  Use the filter and sort parameters to get exactly what you need and pay attention to the limit and max page setting. 

Perform a Preview:

1. On the **Manage REST Collectors** page, click **Run** beside your new Collector.
2. In the resulting **Run Collector** modal, select **Mode > Preview**.
4. Click **Run** to retrieve Preview results.

Automate your collection by clicking the **Schedule** button next to your new collector.
   
## Author
Jim Apger - japger@cribl.io
