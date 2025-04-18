# Push Security REST Collector 

This collector template allows you to collect the Findings from your Push Security deployment. 

## Configuring 

There are two key steps to getting your Push Security collector set up. First we will want to set up the event breaker. 

### Event Breaker 

These steps will allow you import the provided event breaker and configure collector to use the `push-api-findings` event breaker ruleset. Please follow the below steps: 

1. Copy the contents of breaker.json
2. Navigate to the Event breaker rules under Processing --> Knowledge in the Worker Group submenu
3. Create a new breaker ruleset
4. Edit as JSON and paste the previously copied JSON into place; save

### Collector 

After the event breaker is set up: 

1. On the Worker Group submenu, navigate to Data > Sources > Collectors > Rest, click **Add Collector**
2. In the configuration modal, click Manage as JSON
3. Paste in the contents of the provided collector.json file, then click ok
4. Once this is done, navigate to the configuration of the collector, you will need to ensure your Collect URL is the accurate endpoint
5. `x-api-key` in the Collect Request Headers must be replaced with the correct authorization token created and obtained from your push security deployment
6. Navigate to the Results section location the event breaker rulesets and ensure your `push-api-findings` event breaker is selected
7. Once appropriately configured, commit & deploy
8. You should schedule a job and provide a relevant time range.


##Author 

Alex Crusco - acrusco@cribl.io
