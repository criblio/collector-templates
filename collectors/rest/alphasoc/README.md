# AlphaSOC REST Collector

This collector template allows you to collect OCSF Detection Findings from the AlphaSOC [REST API](https://docs.alphasoc.com/api/).

Cribl specific documentation from AlphaSOC is maintained by them [here](https://docs.alphasoc.com/destinations/cribl/).

You will need the **AlphaSOC API Key** from the AlphaSOC console to complete the integration.

## Downloading Configuration Files

Download the required event breaker and REST Collector JSON configuration files from [this Cribl repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/alphasoc). You will import these in the below steps. 

## Import the Event Breaker

The Event Breaker splits each of OCSF Detection Finding into separate record.

1. Navigate to **Manage > Processing > Knowledge > Event Breaker Rules**.
2. Click **Add Ruleset**.
3. Click **Manage as JSON** at lower left.
4. Click **Import** at top right.
5. Select the breaker.json file you downloaded from the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/alphasoc).
6. Click **OK**.

## Importing and Configuring each REST Collector

From the top nav of a Cribl Stream instance or Group, select **Data > Sources**, then select **Collectors > REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST > New Collector** modal.

1. Click **Configure as JSON** to open the configuration editor.
2. Select **Import** from the top right and choose the collector.json file you downloaded from the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/alphasoc).
3. Click **OK**.
4. A modal will open and prompt you for the `API Key` value you captured from the AlphaSOC console. 
5. Click **Replace Values**.
6. Click **OK**.
7. Click **Save**.

The automated collection should be enabled by default indicated by the green **Scheduled** button next to the new collector.

## Testing
Perform a Preview:

1. On the **Manage REST Collectors** page, click **Run** beside your new Collector.
2. In the resulting **Run Collector** modal, select **Mode > Preview**.
3. Optionally specify the **Earliest** value in **Time range**.
4. Click **Run** to retrieve Preview results.


Note: The AlphaSOC REST API supports the ability to pass an **earliest** value.  Any **latest** value assignments are ignored.
