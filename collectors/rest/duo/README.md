# Cribl REST Collector Configuration for the Duo API

## Admin API

Check out the docs for the admin API [here](https://duo.com/docs/adminapi#authentication).

The Duo Admin API uses a basic authentication scheme combined with an HMAC signature. The HMAC signature, generated using a shared secret key, adds an extra layer of security. In order to connect to the API with a Cribl REST Collector, you'll need to be running Stream version 4.8.0 or later.

### Prerequisites

To use the Duo Admin API, you'll need some information. Refer to [First Steps instructions](https://duo.com/docs/adminapi#first-steps) to create an Admin API application. This will give you your`integration key`, `secret key`, and `API hostname` - you'll need these later!

### Configuring the HMAC Function in Cribl Stream

1. In Cribl Stream, navigate to the Worker Groups page, select a Worker Group to configure.
2. On the Worker Groups submenu, navigate to **Group Settings** > **Security** > **Secrets**.
3. On the **Secrets** page, click **Add Secret** to create the following secrets.
    - **duo-admin-secret-key**: Create a new text secret `duo-admin-secret-key` with the value of your Duo `secret key`.
    - **duo-admin-integration-key**: Create a new text secret `duo-admin-integration-key` with the value of your Duo `integration key`.
4. On the Worker Groups submenu, navigate to the **Processing** > **Knowledge** > **HMAC Functions** page.
5. Click **Add HMAC Function** to create a new HMAC function. 
6. On the configuration modal, click **Manage as JSON** and paste in the contents of `hmac_conf_CRIBL-27682_workaround.json`.
    - Note that this configuration includes a workaround for CRIBL-27682. If on Stream version 4.9.0 or later, use `hmac_conf.json` instead.

### Configuring the REST Collector

The same steps below apply to both the `v1_users_collector.json` and the `v2_logs_activity_collector.json` collector configs.

1. On the Worker Groups submenu, navigate to **Data** > **Sources** > **Collectors** > **REST**, click **Add Collector**.
2. In the configuration modal, click **Manage as JSON**.
3. Paste in the contents of the selected 'collector.json' file, then click **OK**.
4. In the prompt asking to **Replace Placeholder Values**, paste your Duo `API hostname` obtained from the Admin API application (e.g., api-xxxxxxxx.duosecurity.com); then click **Replace Values**.
5. Expand the **Authentication** section, and select the configured the HMAC function from the dropdown.
6. Save and Run the Collector.
    - For the logs activity Collector, make sure that you set a value for `earliest` in the **Run Collector** modal to define the starting point for data collection.

### Notes

- REST Collector updates (parameters, methods) might require HMAC function adjustments. Refer to the Admin API's [authentication documentation](https://duo.com/docs/adminapi#authentication) for signature string format details.
- Enable debug logs for Collector runs and search for `Generated HMAC header` to inspect the signature string. To view the generated header, set `authentication` in the **Safe headers** field for the REST collector.
