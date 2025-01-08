# Netskope DataExport (v2) REST API Collector

REST collector to pull events and alerts from the Netskope v2 API. These are also known as the dataexport endpoints.

These two collectors are built from the Netskope Community post: [Cribl Netskope Events and Alerts Integration](https://community.netskope.com/additional-discussions-9/cribl-netskope-events-and-alerts-integration-1246)

More information about the dataexport endpoints can be found on Netskope's documentation: [Using the REST API v2 
`dataexport` Iterator Endpoints](https://community.netskope.com/additional-discussions-9/cribl-netskope-events-and-alerts-integration-1246)

## Configuring
* Ensure you have a `Netskope-Api-Token` created with the appropriate permissions to use the endpoints (i.e. `/api/v2/events/dataexport/events/...` and `/api/v2/events/dataexport/alerts/...`)
* Import the Collector templates and update the placeholders:
  * Subdomain - the Netskope subdomain for your organization. E.g. `goat.goskope.com`
  * `index` - The dataexport iterator index. This value will provide a unique checkpointer for Netskope to use when calling the `next` iterator. You must create a unique value for your REST API Collector. Reusing a value will result in under or over-collection.
  * Netskope API Token - The `Netskope-Api-Token` value.
* Import the provided event breaker.

### Event Breaker
Import the included event breaker to configure the JSON Array event breaking.

1) Copy the contents of breaker.json
2) Navigate to Event Breaker Rules under Processing -> Knowledge
3) Create a new breaker ruleset
4) Edit as JSON and paste the previously copied JSON into place; save
5) Commit and Deploy

## Author
Brendan Dalpe <bdalpe@gmail.com>
