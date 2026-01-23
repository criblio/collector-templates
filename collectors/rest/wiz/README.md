# Wiz REST Collector

This README covers how to configure Cribl Stream REST Collectors and Event Breakers to gather data from the Wiz cloud security platform. To do this, the REST Collectors will communicate with APIs that your organization's Wiz portal exposes. You'll create and configure four REST Collectors, one for each of these Wiz APIs: Audit Logs, Configuration Findings (sometimes called Cloud Configuration), Issues, ,Vulnerabilities and Detections.

When creating REST Collectors, you'll replace a few values in the configuration with values from your Wiz portal. The Manage as JSON approach is faster and less error-prone than clicking your way through the relevant UIs, especially since the configurations required to interact with the Wiz APIs are quite complicated.

You will need the below values from your Wiz portal to complete the integration:

| Item | Value |
| ----------- | ----------- |
| Client ID | 53-character string |
| Client Secret | 64-character string |
| Auth URL | https://auth.app.wiz.io/oauth/token or similar |
| API Endpoint | https://api.us17.app.wiz.io/graphql or similar |

## Downloading Configuration Files

Download the required event breaker and REST Collector JSON configuration files from [this Cribl repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/wiz).  You will import these in the below steps.  

## Configuring the Event Breaker

Although you will create five REST Collectors, you only need to import a single Event Breaker included in the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/wiz), whose ruleset contains one rule per API. All four REST Collectors will use that Event Breaker. Each Wiz REST Collector configuration contained in the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/wiz) will already be configured to use the event breaker (breaker.json) also contained in the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/wiz).

The Event Breaker strips headers from events, leaving only the records of interest; and, it captures timestamps properly.  You will have a single event for each unique record.

1. Navigate to **Manage > Processing > Knowledge > Event Breaker Rules**.
2. Click **Add Ruleset**.
3. Click **Manage as JSON** at lower left.
4. Click **Import** at top right.
5. Select the breaker.json file you downloaded from [here](https://github.com/criblio/collector-templates/tree/main/collectors/rest/wiz).
6. Click **OK**.

## Importing and Configuring each REST Collector
>You'll need to complete the procedure in this section for each of the 5 Wiz Collectors contained in the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/wiz).

>To learn more about REST Collectors, see our [Collector Scheduling and Running page](https://docs.cribl.io/stream/collectors-schedule-run/).

>To learn more about using placeholders in the Collector Import process, see [this](https://docs.cribl.io/stream/collectors/#importing-json).

From the top nav of a Cribl Stream instance or Group, select **Data > Sources**, then select **Collectors > REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST > New Collector** modal.

1. Click **Manage as JSON** to open the configuration editor.
2. Select **Import** from the top right and choose one of the 4 configuration files (named collector-wiz-*.json) you downloaded from the [repo](https://github.com/criblio/collector-templates/tree/main/collectors/rest/wiz).
3. Click **OK**
4. A modal will open and prompt you for the four values provided to you by Wiz (that are specific to your Organization). Populate the form fields for `login_url` (that Wiz calls **Auth URL**), `collect_url` (that Wiz calls **API Endpoint**), `client_secret_value` (that Wiz calls **Client Secret**), and `client_id_value` (that Wiz calls **Client ID**). Tooltips are available for each field, and you can add or modify them later if you prefer.
5. Click **Replace Values**.
6. Click **OK**.
7. Click **Save**.

Once you've created all four REST Collectors, you'll be ready to start collecting from the Wiz APIs.

## Discovering and Collecting
You can perform the procedures in this section with any of the four REST Collectors you've created. To configure time ranges, you'll use **Earliest** and **Latest** values.

- If you enter **Earliest** and **Latest values**, Cribl Stream will pass them into the API query.
- If you leave these fields blank, Cribl Stream will query the API for 24 hours before the job is scheduled to run.

We'll start with the Discovery run:

1. On the **Manage REST Collectors** page, click **Run** beside your new Collector.
2. In the resulting **Run Collector** modal, select **Mode > Discovery**.
3. Configure a **Time range**, if desired.
4. Click **Run** to retrieve Discovery results.

After inspecting these results, launch the Collector run:

1. Back on the **Manage REST Collectors** page, again click **Run** beside your new Collector.
2. In the resulting **Run Collector** modal, this time select** Mode > Full Run**.
3. Configure a **Time range**, if desired.
4. Click **Run**.

### Automating your Collector Runs
1. On the **Manage REST Collectors** page, click **Schedule** in the **Actions** column for your new Collector. The **Run** Collector modal will open.
2. Toggle **Enable** on and configure the scheduling options as desired.
3. Click **Save**.

## Versions
### Version 1.0 
- 2023
- initial release
### Version 1.1
- Jan 23 2026
- Added the Detections collector and modified the exiting event breaker to account for this new collector.
- Modified the event breaker for the Vulnerabilities collector to set the _time field to capture updatedAt instead of createdAt.  Also modified the Vulnerabilities collector with that same change from createdAt to updatedAt.  This prevents duplicate data for each scheduled job focusing on returning only what has updated.

## Author
Jim Apger - japger@cribl.io
