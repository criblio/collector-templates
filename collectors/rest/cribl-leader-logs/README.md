# Cribl Leader Logs Collector

There are some types of logs that are not included in the Cribl Internal Logs source. Use these REST Collector templates to collect logs via the API to close this gap.  You can use the Cloud or On-prem style depending on your deployment.

## Installation

1) Copy the contents of cribl-leader-logs-breaker.json
2) Navigate to Event Breaker Rules under Processing -> Knowledge
3) Create a new breaker ruleset
4) Edit as JSON and paste the previously copied JSON into place; save
5) Copy the contents of either cribl-cloud-leader-logs.json or cribl-onprem-leader-logs.json as required
6) Navigate to Data -> Sources -> REST Collectors
7) Add new
8) Configure as JSON tab (at the top of the window)
9) Paste the JSON from collector into the screen and save
10) Enter the requested values when prompted
11) Commit and Deploy
12) Create your 

## Configuration

The default config is probably fine. You can update the allow list of files eligible for collection by opening the Discover section and modifying the Format discover result test entry box. Specifically you'll need to modify this section:

```
/**
 * Explicit allow list of log filenames eligible for collection.
 *
 * To disable collection for a specific file, comment it out or remove it.
 * Matching is performed against the basename of the item's `id`.
 */
const FILE_ALLOW_LIST = new Set([
  "access.log",            // API request logs
  "audit.log",             // File operations: create/update/commit/deploy/delete
  "credentials-audit.log", // Credential-related audit events
  "cribl_stderr.log",      // Node.js backend stderr output (troubleshooting)
  "cribl.log",             // Primary Cribl application log
  "metrics.log",           // Per-minute ingestion/request metrics
  "notifications.log",     // UI notification events
  "search_metrics.log",    // Search performance metrics
  "searches.log",          // Search execution logs
  "ui-access.log",         // UI route/component access logs
]);
```

## Author
Jeremy Prescott <jprescott@cribl.io>
