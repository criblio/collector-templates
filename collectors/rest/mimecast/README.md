# Mimecast REST API Collector

This collector template allows you to collect logs from Mimecast. 
The collector is using the Mimecast v2 API, documentation for which can be found here:

[Mimecast API Overview](https://developer.services.mimecast.com/api-overview)

[Mimecast API Reference](https://developer.services.mimecast.com/apis)

## Pre-requisites
You will need to create an API application, in order to retrieve a client ID and secret for the OAuth authentication flow.

Mimecast have an article [here](https://community.mimecast.com/s/article/api-integrations-managing-api-2-0-for-cloud-gateway) that guides you through the steps of creating an API application

## Configuring

Create a new REST Collector, then click the Edit as JSON option. Paste the JSON into the screen.

You must replace the following placeholders in the configuration:
* `Client ID` - The client ID of your Mimecast application
* `Client Secret` - The client secret of your Mimecast application

Ensure you schedule this job and provide a relevant time range. The recommendation is to use the following:

Earliest: `-10m@m`
Latest: `-5m@m`

### Event Breaker

Import the provided event breaker (Mimecast).

   - Processing -> Knowledge -> Event Breakers
   - Add new ruleset -> Manage as JSON
   - Paste the contents of breaker.json into the window
   - Save, and ignore the warning about using a filter with `true`

# Filtering the events
## Query parameter
With some Mimecast event sources, you can specify an optional `query` parameter to use for filtering the logs you want to be ingested into Cribl.

Note that this is only supported with the following collectors:
* Mimecast_AuditEvents
* (More collectors to be added in future)

In order to filter the events for these collectors, add the following to the POST body of the collector:
`"query": "update or delete"`

In the case of the [Audit events](https://developer.services.mimecast.com/docs/auditevents/1/routes/api/audit/get-audit-events/post) API, the `query` parameter is described as: "A character string to search for the audit events."

### Query Parameter Example
So if your POST body previously looked like this:
```
`{ ${__e['meta.pagination.next'] ? `"meta":{"pagination":{"pageToken": "${__e['meta.pagination.next']}", "pageSize": 50}},` : ""} "data": [{"endDateTime": "${C.Time.strftime(latest,'%Y-%m-%dT%H:%M:%SZ')}", "startDateTime": "${C.Time.strftime(earliest,'%Y-%m-%dT%H:%M:%SZ')}"}]}`
```

It should now look like this:
```
`{ ${__e['meta.pagination.next'] ? `"meta":{"pagination":{"pageToken": "${__e['meta.pagination.next']}", "pageSize": 50}},` : ""} "data": [{"query":"policy update", "endDateTime": "${C.Time.strftime(latest,'%Y-%m-%dT%H:%M:%SZ')}", "startDateTime": "${C.Time.strftime(earliest,'%Y-%m-%dT%H:%M:%SZ')}"}]}`
```

## Other Parameters
The available options differ depending on the collector/Mimecast API endpoint in use. The Audit Events API endpoint also offers a `category` field for filtering the events to specific categories.

Refer to the [Mimecast API reference](https://developer.services.mimecast.com/apis) documentation for more information on filtering.

## Author
Daniel Harvey - [GitHub](https://github.com/snags141)
