# Wiz REST Collector

This README covers how to configure Cribl Stream REST Collectors and Event Breakers to gather data from the Wiz cloud security platform. To do this, the REST Collectors will communicate with APIs that your organization's Wiz portal exposes. You'll create and configure four REST Collectors, one for each of these Wiz APIs: Audit Logs, Configuration Findings (sometimes called Cloud Configuration), Issues, and Vulnerabilities.

When creating REST Collectors, you'll replace a few values in the configuration with values from your Wiz portal. The Manage as JSON approach is faster and less error-prone than clicking your way through the relevant UIs, especially since the configurations required to interact with the Wiz APIs are quite complicated.

You will need the below values from your Wiz portal to complete the integration:

| Item | Value |
| ----------- | ----------- |
| Client ID | 53-character string |
| Client secret | 64-character string |
| Auth URL | https://auth.app.wiz.io/oauth/token or similar |
| API endpoint | https://api.us17.app.wiz.io/graphql or similar |

## Configuring the Event Breaker

Although you will create four REST Collectors, you only need to import single Event Breaker included in this repo, whose ruleset contains one rule per API. All four REST Collectors will use that Event Breaker. We'll configure each REST Collector to use the rule that corresponds with the appropriate API.

The Event Breaker strips headers from events, leaving only the records of interest; and, it captures timestamps in the proper way.

1. Navigate to Manage > Processing > Knowledge > Event Breaker Rules.
2. Click Add Ruleset.
3. Click Manage as JSON at lower left.
4. Copy the Event Breaker from the Appendix below.
5. Paste the Event Breaker into the editor (replacing the default object that the editor opens with).
6. Click Save.

## Configuring the REST Collectors
>You'll need to complete the procedure in this section four times. Each time, you'll create one REST Collector to handle a particular Wiz API.

>To learn more about REST Collectors, see our REST/API Endpoint and Scheduling and Running topics.

From the top nav of a Cribl Stream instance or Group, select **Data > Sources**, then select **Collectors > REST** from the **Data Sources** page's tiles or the **Sources** left nav. Click **Add Collector** to open the **REST > New Collector** modal.

1. Click **Manage as JSON** to open the configuration editor.

2. Copy the config for the desired API from the desired collector included in this repo.

3. Paste the config into the editor (replacing the default object that the editor opens with).

4. Locate the `collector` element, and within that, the `conf` element.

5. In the `conf` element, replace the values of the following keys with the values you noted earlier. To make this easier, the values to be replaced all have the same placeholder value of `<insert_value_here>`.

    - Replace the value of `loginUrl` with the value of **Auth URL**. Enclose this value in single quotation marks within the double-quotes required by JSON, for example: `"'https://auth.app.wiz.io/oauth/token'"`.
    - Replace the value of `clientSecretParamValue` with the value of **Client secret**.
    - In the `authRequestParams` array, find the object where `name` has the value `client_id`, and replace the value of `value` with the value of **Client ID**.
    - Replace the value of `collectUrl` with the value of **API endpoint**. Enclose this value in single quotation marks within the double-quotes required by JSON, for example: `"'https://api.us17.app.wiz.io/graphql'"`.
6. Click **OK**.

Once you've created all four REST Collectors, you'll be ready to start collecting from the Wiz APIs.

## Discovering and Collecting
You can perform the procedures in this section with any of the four REST Collectors you've created. To configure time ranges, you'll use **Earliest** and **Latest** values.

- If you enter **Earliest** and **Latest values**, Cribl Stream will pass them into the API query.
- If you leave these fields blank, Cribl Stream will query the API for the 24 hours before the job is scheduled to run.

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

## Author
Jim Apger - japger@cribl.io
