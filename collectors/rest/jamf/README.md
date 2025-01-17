# JAMF Computers

This collector template allows you to retrieve computer information from JAMF

# Installation

1) Navigate to Event Breaker Rules under Processing -> Knowledge
1) Create a new breaker ruleset
1) Edit as JSON and paste the contents of breaker.json. Save
1) Navigate to Data -> Sources -> REST Collectors
1) Add Collector
1) Configure as JSON tab (at the top of the window)
1) Paste the JSON from collector.json into the screen and save
1) You will be prompted for the server:port, username and password. When done, hit Replace variables.
1) Navigate to Processing -> Pipelines
1) Create a Pipeline, edit as JSON, and paste the contents of pipeline.json. Save.
1) Navigate to Routing -> Data Routes
1) Create a Route with filter: `logtype == 'jamf:computers'`, point it to the jamf-computers-xml-unroll pipeline and your desired destination. Save.
1) Commit and Deploy
    
## Further configuration

You can be more specific with the retrieval by adding a search id to the collection URL, example:

`https://yourJamfServer:8080/JSSResource/advancedcomputersearches/id/2129`

If you want to run this periodically, configure a schedule. There is not one by default.

## Authors
Basic working sample provided by community member Mehdi -- thanks Mehdi!  
Curated by `Jon Rust <jon@cribl.io>`
