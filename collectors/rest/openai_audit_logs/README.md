# Boomi REST Collector

This collector template allows you to collect logs from Boomi's api.

# Installation

1) Copy the contents of breaker.json
2) Navigate to Event Breaker Rules under Processing -> Knowledge
3) Create a new breaker ruleset
4) Edit as JSON and paste the previously copied JSON into place; save
5) Copy the contents of collector.json
6) Navigate to Data -> Sources -> REST Collectors
7) Add Collector
8) Configure as JSON tab (at the top of the window)
9) Paste the JSON from collector into the screen and save
10) You will be prompted for the bearer token. Enter and save

## Notes
* It's set to run every 15 minutes by default. Update as you require.
* The openAI API limits the response to 100 events. If you anticipate more, than that to catch up, you may need to to make manual "full runs". be sure to enable state tracking on each run.
    * If the response is being limited by the 100 cap on every run, increase the run frequency (eg, 15 minutes to 5 minutes)
* The EB config is anticipating more openai breakers to come in the future
    * Each would be named for the endpoint, as this one is named `audit_log`
    * The resulting cribl_breaker value would then be `openai:audit_log` making rules and filters easy to match
* If state is not available, the collector will use earliest, as provied in the run interface. If earliest also is not abailable. the collector will use -24h
* State is using the effective_at time stamp in each event, saving hte highest value it's seen.
    * The event IDs might be a better option, but current limitations in the collector logic are complicating that. Hopefully we can find a way around it and make this work.

## Author
Jon Rust <jon@cribl.io>
