# AlphaSOC REST Collector - Findings

This collector template allows you to collect logs from the AlphaSOC [REST API](https://docs.alphasoc.com/api/).

Cribl specific documentation from AlphaSOC is maintained by them [here](https://docs.alphasoc.com/destinations/cribl/).

You will need the below values from your AlphaSOC portal to complete the integration:

| Item | Value |
| ----------- | ----------- |
| API Key | 60-character string |

## Downloading Configuration Files

Download the required event breaker and REST Collector JSON configuration files from [this Cribl repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/alphasoc_findings).  You will import these in the below steps. 

## Import the Event Breaker

The Event Breaker strips headers from events, leaving only the records of interest and capturing timestamps properly.  You will have a single event for each unique record.

1. Navigate to **Manage > Processing > Knowledge > Event Breaker Rules**.
2. Click **Add Ruleset**.
3. Click **Manage as JSON** at lower left.
4. Click **Import** at top right.
5. Select the breaker.json file you downloaded from the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/alphasoc_findings).
6. Click **OK**.

## Importing and Configuring each REST Collector

From the top nav of a Cribl Stream instance or Group, select **Data > Sources**, then select **Collectors > REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST > New Collector** modal.

1. Click **Configure as JSON** to open the configuration editor.
2. Select **Import** from the top right and choose the collector.json file you downloaded from the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/alphasoc_findings).
3. Click **OK**.
4. A modal will open and prompt you for the two values you captured from your Falcon portal. Populate the form fields for `clientSecretParamValue_value` , and `client_id_value`. 
5. Click **Replace Values**.
6. Click **OK**.
7. Click **Save**.

## Testing
The AlphaSOC REST API does not yet support passing of earliest/latest time values into the query.  The initial query will capture 3 days worth of data then Cribl Stream state tracking will ensure incremental updates after that.  

Perform a Preview:

1. On the **Manage REST Collectors** page, click **Run** beside your new Collector.
2. In the resulting **Run Collector** modal, select **Mode > Preview**.
3. No **Time range** values are used.
4. Click **Run** to retrieve Preview results.

Automate your collection by clicking the **Schedule** button next to your new collector.
   
## Author
Jim Apger - japger@cribl.io
