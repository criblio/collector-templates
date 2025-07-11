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


# advanced setup : 
## Need to adjust for lagging REST Collector
If your data collection is lagging sometimes, it could be that DDOS attack are flooding with logs. the akamai api being limited in troughput, you can't keep up with the volume during an attack. if you want to skip ahead whenever the data collection is lagging.
- use the rest configuration *rest_akamai_offset_advanced.json* 
This collection is still mono threaded, and still using offset (which is the best way to make sure every event is collected exactly once), but state tracking will know when colleciton is behind, and will skip ahead when it lags. This will allow you to skip when akamai has a burst of logs.
- Note : this data collection is monothreaded as the offset is sent last by akamai, so it's limited to 2000-4000eps maximum.
1.Reproduce steps as in *Install the REST Collector* chapter to use it.
2.monitor your lag 


## Need for high volume collection
If your akamai id sends more than 3000 eps, you might want to use the multitread configuration, which uses to/from setup instead of offset. 
This collection can ingest a lot more logs (7 millions EPS with 60 threads, in theory). Since it doesn't use offset, it cannot guarantee you get every logs (if the job fails for example).
Note that this configuration can only handle 1 id, which you set directly in the url.
- Use the file : *rest_akamai_timestamp_advanced.json*
this file uses 6 threads, one per 10 second request, 1 job per minute (720k EPS in theory with 2000 eps per thread). It will auto skip ahead if it lags beind thanks to state tracking.
1. To use it reproduce steps as in *Install the REST Collector* chapter BUT : 
2. replace the xxx in the collectUrl with your id

## Monitor the collection lag and duplicates
monitor the data ingestion lag : this pipeline will use now as ingestion time, and event time to calcultate the lag. Efectively changing the _time of event to visually see the event by the time in came in. It also create a sha field from the time and the raw log to detect duplicates wihtout sending sensitive information. 
1. use monitor_collector.json as a pipeline, in cribl stream->worker group->pipeline->add pipeline->manage as json. paste the json.
2. create a route toward a destination of your choice (Cribl Lake), 
3. monitor the lag (in cribl search or else) with this search : *dataset="mydataset" input="collection:xxx" |timestats span=5s count(), max(lag), min(lag), percentile(lag,90)* 
4. monitor the duplicates with this search *dataset="xxx"  | summarize count() by sha | where count_ > 1*

# Author
Sidd Shah - sshah@cribl.io
