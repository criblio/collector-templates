# 1Password REST Collector with State Tracking for Audit logs

This uses the 1Password Events API to retrieve audit logs from your 1Password account so you can send them to your SIEM or object storage for long-term search or retention.

1Password Events API reference [here](https://developer.1password.com/docs/events-api/reference)

You will need the below values to complete the integration.  See the above API reference for your Base URL.

| Item | Value |
| ----------- | ----------- |
| Base URL | URL from the API Reference |
| Authorization Token | API token obtained from your 1Password account |

## Downloading Configuration Files

Download the REST Collector JSON configuration files from [this Cribl repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/1Password_with_state_tracking).  You will import these in the below steps. 

## Import the Event Breaker

The Event Breaker strips headers from events, leaving only the records of interest and capturing timestamps properly.  You will have a single event for each unique record.  The event breaker is configured to set the timestamp equal to the 'timestamp' field within each event.

1. Navigate to **Manage > Processing > Knowledge > Event Breaker Rules**.
2. Click **Add Ruleset**.
3. Click **Manage as JSON** at lower left.
4. Click **Import** at top right.
5. Select the breaker.json file you downloaded from [here](https://github.com/criblio/collector-templates/tree/main/collectors/rest/1Password_with_state_tracking).
6. Click **OK**.

## Import and Configure the REST Collector

From the top nav of a Cribl Stream instance or Group, select **Data > Sources**, then select **Collectors > REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST > New Collector** modal.

1. Click **Configure as JSON** to open the configuration editor.
2. Select **Import** from the top right and choose the collector.json file you downloaded from the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/1Password_with_state_tracking).
6. Click **OK**.
7. Enter the value for **Collect URL** based on info in the above API reference.  Example:  'https://events.1password.com/api/v2/auditevents'.
8. Enter the value for the Collect header field named **Authorization**.  Example:  'Bearer eYk4hkj3hk3h423424<truncated>' 
9. Click **Save**.

## Testing

Perform a Preview:

1. On the **Manage REST Collectors** page, click **Run** beside your new Collector.
2. In the resulting **Run Collector** modal, select **Mode > Preview**.
3. You must populate the **Earliest** time field.  If you leave the **Latest** time field blank, it will default to **now**.
4. Click **Run** to retrieve Preview results.

## Pagination

The 1Password API requires the first query to provide the **limit** and **start_time** objects (**end_time** is optional but implemented in this collector) within the HTTP body.  If more records need to be returned than the configured limit,  **cursor** and **has_more** fields are returned and need to be used in each successive query until **has_more** equals false.

## Scheduling and State Tracking

Automate your collection by clicking the **Schedule** button next to your new collector.  This collector uses the default values for the state related expressions under **State Tracking** because the **1Password** event breaker extracts the time property from each record and assigns that value to the Cribl Stream **_time** field.
   
## Author
Jim Apger - japger@cribl.io

