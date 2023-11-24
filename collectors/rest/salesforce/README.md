# Salesforce REST Collector

This README covers how to configure Cribl Stream REST Collectors and Event Breakers to gather data from the Salesforce APIs. To do this, the REST Collectors will communicate with Salesforce APIs exposed by your organization.

## Configuring the Event Breaker

Although you will create REST Collectors, you'll only need to import a single Event Breaker included in this repo, containing rulesets corresponding to various Salesforce APIs.

### Configuration Values Required

| Item | Value |
| ----------- | ----------- |
| Client ID | 53-character string |
| Client secret | 64-character string |
| Auth URL https://your-salesforce-instance.com/services/oauth2/token or similar |
| API endpoint https://your-salesforce-instance.com/services/data/v58.0/query?q=SELECT,SourceIp,Status,UserId+FROM+LoginHistory or similar query from another object [Salesforce Objects](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_list.htm) |

## Configuring a Connected App

To communicate with Salesforce APIs, you need to set up a connected app in your Salesforce organization:

1. **Log in to Salesforce**
    - Log in to your Salesforce instance.

2. **Navigate to Setup**
    - Go to Setup > Apps > App Manager.

3. **Create a New Connected App**
    - Click 'New Connected App'.
    - Fill in the required details such as app name, API settings, and callback URL.
    - Make sure to note down the generated `Client ID` and `Client Secret`.

4. **Configure OAuth Settings**
    - Set OAuth scopes and other settings as needed for your integration.

5. **Obtain OAuth Information**
    - Retrieve the `Client ID` and `Client Secret` generated for your connected app. These values will be used in the Cribl Stream configuration.

## Configuring the REST Collectors

1. **Retrieve Collector Configuration**
    - Obtain the collector configuration for the desired Salesforce API from the repository.

2. **Edit Collector Configuration**
    - Replace placeholders in the configuration with the values obtained from your Salesforce instance, including the `Client ID` and `Client Secret` from the connected app.

3. **Create and Configure REST Collectors**
    - Repeat the procedure for each specific Salesforce API.

## Discovering and Collecting

### Discovery Run
1. Run the Collector in **Discovery** mode to retrieve initial results.
2. Review the results and proceed.

### Full Collector Run
1. Run the Collector in **Full Run** mode after reviewing the Discovery results.

## Automating Collector Runs
1. Schedule the Collector runs as needed for automated data collection.

## Author
Algi Tabir - contact@algi.dev
