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
If your data collection is lagging, it could be that your collection is too voluminous or a DDOS attack is flooding with logs. This can lead to data loss, inaccurate analysis, and potential security risks. The Akamai API being limited in throughput (approximately 2500-3000 EPS), you can't keep up with the volume during an attack. To solve this : skip ahead whenever the data collection is lagging.
- use the rest configuration *rest_akamai_offset_advanced.json* 
This collection is mono-threaded, just like the standard one, and still uses offset (which is the best way to ensure every event is collected exactly once). However, state tracking will detect when the collection is behind and will skip ahead when it lags. Jumping ahead will allow you to skip a burst of logs and avoid lagging. The drawback is that your data collection will be incomplete. completeness VS Recency : Akamai API only allows you to pick one.
[Link to State tracking doc](https://docs.cribl.io/stream/collectors-rest/#state-tracking)
- Note: this data collection is monothreaded as the offset is sent last by Akamai, so it's limited to 2000-4000eps maximum.
1. Reproduce steps as in *Install the REST Collector* chapter to use it.
2. Monitor your lag to verify that most of the time your lag is OK (below 90s typically). (There are multiple ways to achieve this; the easiest is just to check your state in your input ->manage state : it will show you the latest event time, and if it's lagging. You can also gather collection stats in Cribl Lake and monitor them in Cribl search with a custom pipeline. If you're unclear, please get in touch with Simon via the email below. I might publish a config on this topic someday.

At this point, if your collection is lagging, your baseline data volume is too high for a single-threaded collection, you need a multithreaded collection. keep reading.


## Need for high volume collection
If your akamai id sends more than 3000 eps, you might want to use the multitread configuration. This method uses a to/from collection and splits every minute into 4 jobs of 15 seconds.
This collection can ingest a lot more logs (8-15000 EPS with 4 threads, in theory). Since it doesn't use offset, it cannot guarantee that you receive every log (for example, if the job fails).
Note that this configuration can only handle 1 id, which you set directly in the url.
- Use the file : *rest_akamai_timestamp_advanced.json*
This file uses four threads, one per 15-second request, with one job per minute. It will auto-skip ahead if it lags, thanks to state tracking.
1. To use it :  reproduce steps as in *Install the REST Collector* chapter BUT : 
2. replace collectURL with your URL AND replace the IDNUMBERHERE at the end with your id
Note that this collection will generate duplicate data. A keen eye will see that Akamai will randomly send you data outside the date boundary, data that you have already collected a minute ago. Disappointed? blame Akamai :) However, don't fret, Cribl Stream can extract data outside the time boundary. To solve this issue got to step 3
3. use pipeline_drop_duplicates.json as a pipeline to drop event where the eventtime is before the time boundary set.
and you are all set.
you may want to check the lag status on the state tracking (the easiest it to just check your state in your input->manage state : it will show you the latest event time. ).
If data collection is lagging constantly, add more threads. To do so, check the discovery itemList to split every minute : 0,15,30,45 means 4 thread of 15 seconds. Having 6 threads can be achieved by having the discovery itemlist set to : 0,10,20,30,40,50 and changing the to and from request parameters to adjust for the 10-second difference instead of 15. But refrain from changing the current config of 4 threads in a minutes, this is a sweet spot.


## Monitor the collection lag and duplicates
Monitor the data ingestion lag : this pipeline will use now as ingestion time, and event time to calculate the lag. Effectively changing the _time of the event to visually see the event by the time it came in. It also creates a sha field from the time and the raw log to detect duplicates without sending sensitive information. 
1. use monitor_collector.json as a pipeline, in cribl stream->worker group->pipeline->add pipeline->manage as json. paste the json.
2. create a route toward a destination of your choice (Cribl Lake), 
3. monitor the lag (in cribl search or else) with this search : *dataset="mydataset" input="collection:xxx" |timestats span=5s count(), max(lag), min(lag), percentile(lag,90)* 
4. monitor the duplicates with this search *dataset="xxx"  | summarize count() by sha | where count_ > 1*

# Author
Sidd Shah - sshah@cribl.io
Simon Duchene - sduchene@cribl.io
