# Cribl REST Collector Configuration for the Duo API

## Admin API

Check out the docs for the admin API [here](https://duo.com/docs/adminapi#authentication).

The Duo admin API uses basic authentication signed using an HMAC signature. In order to connect to the API with a Cribl REST Collector, you'll need to be running Stream version 4.8.0 or later.

### Prerequisites

Set up the Admin API application within Duo using the [First Steps instructions](https://duo.com/docs/adminapi#first-steps) to get your `integration key`, `secret key`, and `API hostname` that will be used below.

### Configuring the HMAC Function

1. From Group Settings > Security > Secrets
    - Create a new text secret `duo-admin-secret-key` with the value of your Duo `secret key`
    - Create a new text secret `duo-admin-integration-key` with the value of your Duo `integration key`
2. Navigate to the Processing > Knowledge > HMAC Functions page
3. Create a new HMAC function by clicking the `Add HMAC Function` button
4. From the configuration modal, click `Manage as JSON` and paste in the contents of `hmac_conf_CRIBL-27682_workaround.json`
    - Note that this configuration includes a workaround for CRIBL-27682. If on Stream version 4.9.0 or later, use `hmac_conf.json` instead.

### Configuring the REST Collector

1. From Data > Sources > Collectors > REST, click the `Add Collector` button.
2. From the configuration modal, click the `Manage as JSON` button
3. Paste in the contents `admin-collector.json`, then click `OK`
4. Fill in your Duo `API hostname` (should look something like `api-xxxxxxxx.duosecurity.com`), then click `Replace Values`
5. Expand the `Authentication` section, and select the configured the HMAC function from the dropdown
6. Save and Run the collector

### Notes

If updates are made to the REST collector (adding parameters, changing the method, etc.), the HMAC function may need to be updated. See the admin API's [authentication documentation](https://duo.com/docs/adminapi#authentication) to see the expected format for the signature string configuration.

Turn on debug logs for the collector run and search for the `Generated HMAC header` for more visibility into the evaluated signature string. To see the generated header, make sure you set `authentication` in the `Safe headers` field for the rest collector.