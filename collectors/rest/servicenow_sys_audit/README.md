# ServiceNow sys_audit REST Collector

## Author
- **Name:** Andrew Hong
- **Email:** andrew.hong@cribl.io
- **Company:** Cribl

## Overview
This template collects records from the ServiceNow `sys_audit` table using the Table API (`/api/now/table/sys_audit`).

## Requirements / Placeholders

Update the following placeholders in `collector.json` before use:

- `<ServiceNow sys_audit URL|Full ServiceNow Table API URL for sys_audit, e.g. https://INSTANCE.service-now.com/api/now/table/sys_audit>`
- `<ServiceNow Username|ServiceNow account used for API access>`
- `<ServiceNow Password|Password or token for the ServiceNow user>`

The collector uses a relative time range with `earliest` set to `-1d` and tracks `latestTime` state based on `sys_created_on`.

## Usage

1. Import `collector.json` into your Cribl Stream environment.
2. Import `breaker.json` into your Event Breaker configuration.
3. Configure any placeholders listed above.
4. Run a test collection and confirm events parse correctly.

## Sample Data

Place anonymized NDJSON samples for `sys_audit` events in the `samples/` directory (one JSON object per line, < 256 KB per file).
