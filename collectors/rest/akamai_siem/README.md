# Akamai Edgegrid

[EdgeGrid Auth Documentation](https://techdocs.akamai.com/developer/docs/authenticate-with-edgegrid#authentication-protocol-specification)

# Configuring

Credentials must be generated in the Akamai Control Center [Documentation](https://techdocs.akamai.com/developer/docs/create-a-client-with-custom-permissions)

# Prepare Cribl Environment

## Add Akamai Credentials to Cribl Stream

*Note*: If a external secret store is utilized, the HMAC function will have to be modified to reference those secrets using the proper names.

1. Open the worker group that will contain the Akamai collector.
2. Navigate to **Group Settings** → **Security** → **Secrets**
3. Create 2 Secrets. 
   1. The first will have type **API key and secret key**. This secret should be named `akamai_client_credentials`. Populate the client ID as the API Key and the client secret as the Secret Key.
   2. The second will have type **Text**. This secret should be named `akamai_access_token`. Populate the Value field with the Akamai Access Token.

## Install the Event Breaker
1. Obtain the event breaker. (*eb_akamai_siem_integration.json*)
2. Navigate to **Processing** → **Knowledge** → **Event Breaker Rules**.
3. Click **Add Ruleset** followed by **Manage as JSON**.
4. Click **Import** and select the event breaker file (*eb_akamai_siem_integration.json*).
5. Click **Ok** and then **Save** to close the modal.

## Install the HMAC Function
1. Obtain the HMAC function. (*hmac_akamai_edgegrid.json*)
2. Navigate to **Processing** → **Knowledge** → **HMAC Functions**
3. Open the **Add HMAC Function** modal, and **Manage as JSON**
4. Click **import** and import the HMAC function JSON (*hmac_akamai_edgegrid.json*)
5. Click **Ok** then **Save** to close to modal.

## Install the Pre-Processing pipeline
1. Obtain the pre-processing pipeline, (*pipe_parse_akamai_siem.json*)
2. Navigate to **Processing** → **Pipelines**
3. Click **Add Pipeline** followed by **Import from File**
4. Select the file downloaded from Github. (*pipe_parse_akamai_siem.json*)
5. Click **Save** to save the pipeline.
6. (Optional) - update any parsing settings in the pipeline to fit your use-case.

## Install the REST Collector
1. Obtain the REST Collector.
2. Navigate to **Data** → **Sources** → **REST Collectors**
3. Click **Add Collector**, then **Manage as JSON**
4. Click **Import** and select the file from GitHub. (*rest_akamai_siem_integration.json*)
5. Click **Ok** to close the JSON view.
6. Update the URL under **Collect URL** replacing the URL as obtained from the Akamai Control Center.
7. Update the list in *Discover* with a list of configIds for this endpoint. More info on configIds can be found [here](https://techdocs.akamai.com/siem-integration/reference/get-configid).
8. Validate the HMAC function is selected in the Authentication section of the Collector Settings page.
9. Validate the pre-processing pipeline is selected under **Result Settings** → **Result Routing**.

# Notes

- The Event Breaker must parse the received payload into JSON to allow state tracking utilizing offset to work. Time stamp based collection does not require this.
- The preprocessing pipeline handles both dropping the state tracking event and handles decoding events.
- Pagination is not supported by the API, and the server will disconnect your session after 1 minute. In this scenario, an offset context is not sent and will result in the collection of the same data multiple times. If collection tasks (individual config IDs) take longer than 1 minute, reduce the limit parameter in the REST collector.

# Author
Sidd Shah - sshah@cribl.io
