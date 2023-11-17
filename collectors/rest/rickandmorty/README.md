# Rick and Morty REST Collector

This collector template allows you to collect character information from the Risk and Morty API (https://rickandmortyapi.com/).  

It was built as a no-cost example of how one might go about capturing user or device context from an API for the purpose of building an identity or asset framework (ex CMDB).  The example includes event breaking and pagination.

## Configuring

From within Stream, select Data->Sources.  Select the Collector/Rest tile.  Click Add Collector then click Manage as JSON.  Paste the contents of the `collector.json` file then click Save.

Since there is no authorization, you do not need to update anything in the config.

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `RickAndMorty` event breaker rule set if not already done.

## Author
Brendan Dalpe - bdalpe@cribl.io
