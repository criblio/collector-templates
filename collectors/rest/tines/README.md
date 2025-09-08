# Tines REST Collectors

This collector template allows you to collect from the Tines REST API:
https://www.tines.com/api/welcome/

# Collectors
tines_event_list - https://www.tines.com/api/events/list/

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
10) Fill in the API key and domain for your account in the prompt.  Validate 
11) Commit and Deploy

## Configuring

The collector is scheduled to run 5 minutes after every hour.  State tracking is used to get events with IDs newer than what we've seen. Ad hoc runs do not use state by default, and can only be enabled in an ad hoc Full Run.

## Notes for usage

Because we parse the JSON at the Event Breaker level, you'll end up with both the original data in _raw, as well as the extracted data at the top level of the event. 

You should clean this up! Don't send both.


## Authors
Ryan Allen <rallen@cribl.io>
