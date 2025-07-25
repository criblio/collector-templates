# README

[Salesforce](https://www.salesforce.com/) provides an [API endpoint to execute a SOQL query](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_query.htm) to get records of the requested object.

Salesforce defines [standard objects](https://developer.salesforce.com/docs/atlas.en-us.sfFieldRef.meta/sfFieldRef/salesforce_field_reference.htm), such as LoginHistory or SetupAuditTrail, whose records are useful in a SIEM pipeline. [EventLogFile](https://developer.salesforce.com/docs/atlas.en-us.sfFieldRef.meta/sfFieldRef/salesforce_field_reference_EventLogFile.htm?q=EventLogFile) is a special case because its records contain [data about event log files](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_event_log_file_query.htm) ready to be downloaded from [a different endpoint](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/dome_event_log_file_blob.htm).

This Cribl REST collector enables a compact way to get:

- Salesforce records using the [Query endpoint](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_query.htm) and
- Salesforce event monitoring content from EventLogFile records using the [sObject Blob Get endpoint](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_sobject_blob_retrieve.htm).

## How to use it

1. Import the Event Breaker Ruleset:
   1. Go to *Processing -> Knowledge -> Event Breaker Rules -> Add Ruleset*.
   2. Click on *Manage as JSON* and paste the content of `breaker.json`.
2. Import the REST collector:
   1. Go to *Data -> Sources -> Collectors -> REST -> Add Collector*.
   2. Go to *Configure as JSON* tab and click on *Import*.
   3. Import the `collector.json` file.
   4. Provide the required values: domain of the customer, API version, user name, password, client ID and client secret.
3. (Optional) Edit the queries on object records in the *format discovery result* code of the collector to suit your needs.
4. Commit and deploy.

## How it works

The basic steps follow the usual workflow in Cribl collectors:

1. Discover the event log files to be downloaded using a HTTP request to get EventLogFile records.
2. Add a static list of discover results for records of other objects that don't require subsequent downloads using *format discover result*.
3. Collect event log files and records using the appropriate URL and event breaker rules.

All files included are meant to be adapted for your specific case. Depending on your needs you can decide to create a collector just for records and just for event log files. They are based on an actual deployment and they were developed by the teams of [Repsol](https://www.repsol.com/) and [Allenta](https://allenta.com/).

### Discovery of event log files

The discovery uses a HTTP GET request to download the EventLogFile records that will be used to create the collect tasks to download the event log files.

Note that the *Discover datafield* in this collector is `records`.

### Usage of format discover result to add other tasks

To get other records such as LoginHistory or SetupAuditTrail, we use code in *format discover result* to add a static list of tasks. This way we can have a compact collector that takes care of everything related to Salesforce.

An example of code is:

```js
// Set a static discovery result of list of Salesforce queries and cursors.
// The cursor is the time field used to set the time range fo the events.
const queries = [
  {
    query: "SELECT LoginHistory.Id, LoginHistory.LoginTime, LoginHistory.UserId, LoginHistory.LoginType, LoginHistory.Status, LoginHistory.Platform, LoginHistory.SourceIp, LoginHistory.CountryIso, LoginHistory.Browser, LoginHistory.Application, LoginHistory.ClientVersion, LoginHistory.APIversion, LoginHistory.LoginUrl FROM LoginHistory",
    cursor: "LoginTime"
  },
  {
    query: "SELECT SetupAuditTrail.Id, SetupAuditTrail.CreatedById,  SetupAuditTrail.DelegateUser, SetupAuditTrail.Action, SetupAuditTrail.Display, SetupAuditTrail.Section, SetupAuditTrail.CreatedDate FROM SetupAuditTrail",
    cursor: "CreatedDate"
  }
];
// Add static discovery results to already obtained.
__e["records"] = (__e["records"] || []).concat(queries);
```

This is a merge of HTTP Request and JSON Response as *Discover type*.

### Collection and event breaker rules

This is an example of a discover data field used to create collect tasks:

```json
[
  // Discovery results from the HTTP Request.
  { "attributes" : { "type" : "EventLogFile",
      "url" : "/services/data/v64.0/sobjects/EventLogFile/0ATD000000001bROAQ"},
    "Id" : "0ATD000000001bROAQ",
    "EventType" : "API",
    "LogFile" : "/services/data/v64.0/sobjects/EventLogFile/0ATD000000001bROAQ/LogFile",
    "LogDate" : "2014-03-14T00:00:00.000+0000",
    "LogFileLength" : 2692.0 },
  { "attributes" : { "type" : "EventLogFile",
      "url" : "/services/data/v64.0/sobjects/EventLogFile/0ATD000000001SdOAI"     },
    "Id" : "0ATD000000001SdOAI",
    "EventType" : "API",
    "LogFile" : "/services/data/v64.0/sobjects/EventLogFile/0ATD000000001SdOAI/LogFile",
    "LogDate" : "2014-03-13T00:00:00.000+0000",
    "LogFileLength" : 1345.0 },
  // Static discovery results added in "format discover result".
  { query: "SELECT LoginHistory.Id, LoginHistory.LoginTime, LoginHistory.UserId, LoginHistory.LoginType, LoginHistory.Status, LoginHistory.Platform, LoginHistory.SourceIp, LoginHistory.CountryIso, LoginHistory.Browser, LoginHistory.Application, LoginHistory.ClientVersion, LoginHistory.APIversion, LoginHistory.LoginUrl FROM LoginHistory",
    cursor: "LoginTime"
  },
  {
    query: "SELECT SetupAuditTrail.Id, SetupAuditTrail.CreatedById,  SetupAuditTrail.DelegateUser, SetupAuditTrail.Action, SetupAuditTrail.Display, SetupAuditTrail.Section, SetupAuditTrail.CreatedDate FROM SetupAuditTrail",
    cursor: "CreatedDate"
  }
]
```

The two types of discover results —event log files and records— require different collect URL and different event breakers:

- For **discovery results related to event log files**, the collect URL `https://foo.my.salesforce.com/services/data/v53.0/sobjects/EventLogFile/${Id}/LogFile` downloads a CSV file with events.
- For **discovery results related to records of objects**, the collect URL `https://foo.my.salesforce.com/services/data/v53.0/query` downloads a JSON object containing a list of records.

The expression to build the correct collect URL for each case is:

```js
'https://foo.my.salesforce.com' +
  LogFile ?
    `/services/data/v53.0/sobjects/EventLogFile/${Id}/LogFile` :
    (nextRecordsUrl || '/services/data/v53.0/query')
```

Also, the event breaker takes into account the CSV or JSON format of the collected events.

## Improvements

[ ] Manage password as a secret using [C.Secret](https://docs.cribl.io/stream/expressions-other/#secret). This should be done manually and it's not grave because the client secret is already encrypted.

## Authors

Tomás García Hidalgo <tomas.garcia.h@repsol.com>
Roberto Moreda <moreda@allenta.com>
