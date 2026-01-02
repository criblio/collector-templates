# Proofpoint CASB REST Collector

This collector template allows you to collect logs from Proofpoint CASB. It uses the extracted time and [Cribl state tracking](https://docs.cribl.io/stream/collectors-manage-state/) to remember where it left off after each successful run.

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
10) If there are fields for you to fill out, you will be prompted here. When done, hit Replace variables.
12) Commit and Deploy

The schedule is disabled by default. Once you validate operation, you'll want to enable this. By default it would run every 5 minutes. Adjust as you see fit.

## Authors
- Chris W (thanks, Chris!)
- Eleane Ye <eye@cribl.io>
- Randy Correlli <rcorelli@cribl.io>
- Jon Rust <jrust@cribl.io>
