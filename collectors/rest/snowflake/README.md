# Snowflake REST Collector for Cribl Stream

## Overview

This repository provides a Cribl Stream REST collector template for ingesting Snowflake audit and query history data via the Snowflake SQL API. It enables you to collect audit data from your Snowflake environment, normalize the events, and forward them to supported analytics platforms or SIEMs.

The collector leverages Cribl Stream’s REST collector framework for flexible data pipeline, normalization, and distribution capabilities.

## Features

- **Collects Snowflake Audit Events and Query History**
- Supports **JWT/OAuth authentication** for secure API access
- Ingests data on a **scheduled or ad-hoc basis**
- **Transforms and normalizes** Snowflake API responses into clean key-value pairs suitable for downstream systems
- Fully compatible with Cribl Stream's **event routing, filtering, and pipeline** tools.

## Prerequisites

- A running Cribl Stream deployment (self-hosted or in Cribl.Cloud)
- A Snowflake account with permissions to query the relevant audit or history tables (for example: `ACCOUNT_USAGE.QUERY_HISTORY`, `ACCOUNT_USAGE.LOGIN_HISTORY`)
- Snowflake SQL API enabled and appropriate network access configured
- An **API integration** and (optionally) an OAuth security integration in Snowflake
- A method to provide a valid **JWT or OAuth token** to Cribl for authentication

See [Snowflake documentation](https://docs.snowflake.com/en/developer-guide/sql-api) for details on preparing your environment.

## Installation

1. **Clone this repository**
    ```sh
    git clone <repo-url>
    ```

2. **Import the Collector Template into Cribl Stream**
   - Go to `Data > Sources` in your Cribl Stream UI
   - Select `REST` under Collectors, then click "Add Collector"
   - Use the provided JSON configuration file for the Snowflake collector template
   - Update placeholders (URLs, tokens, etc.) as needed for your environment

## Configuration

### Snowflake SQL API Setup

Before configuring the collector, ensure the following in your Snowflake environment:

- Grant network access to your Cribl instance(s) using the Snowflake network policy
- Create an API Integration for programmatic access
- (Optional) Create an OAuth Security Integration for dynamic token refresh
- Retrieve or generate your JWT or OAuth access token.

### Collector Settings

The provided template uses these settings as a baseline:

| Setting                  | Example Value                                                      | Notes                                                                |
|--------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------|
| Input ID                 | `snowflake_audit_login_history`                                    | Unique for each collector                                            |
| Description              | Collects Snowflake login history events                            | Human-readable description                                           |
| Discover                 | None                                                               | No discovery step needed for static queries                          |
| Collect URL              | `https://<account>.snowflakecomputing.com/api/v2/statements`       | Append `/api/v2/statements` to your account URL                      |
| Collect Method           | POST with Body                                                     | Use POST to submit SQL statements                                    |
| Collect POST Body        | `{ "statement": "SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.LOGIN_HISTORY WHERE EVENT_TIMESTAMP >= DATEADD(hour, -1, CURRENT_TIMESTAMP())" }` | This example fetches the last hour of login events                   |
| Collect Headers          | `Content-Type: application/json`, `Authorization: Bearer <token>`  | Use a valid access or bearer token                                   |
| Pagination               | None                                                               | The SQL API returns all matching events for the time window          |
| Authentication           | None*                                                              | (Token is supplied as a header; use "None" auth mode in Cribl)       |
| Polling Interval         | `300s` (every 5 minutes)                                           | Adjust as needed                                                     |

*For OAuth, you may configure Cribl-managed secrets or a custom authentication flow using the REST collector's advanced options.

#### Sample Collector Configuration (JSON snippet)
```json
{
  "inputId": "snowflake_audit_login_history",
  "description": "Collects Snowflake login history events",
  "discover": { "type": "none" },
  "collect": {
    "url": "https://<account>.snowflakecomputing.com/api/v2/statements",
    "method": "POST_WITH_BODY",
    "body": {
      "statement": "SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.LOGIN_HISTORY WHERE EVENT_TIMESTAMP >= DATEADD(hour, -1, CURRENT_TIMESTAMP())"
    },
    "headers": {
      "Content-Type": "application/json",
      "Authorization": "Bearer ${token}"
    }
  },
  "schedule": { "interval": 300 }
}
```
Be sure to adjust the collect body and headers for your SQL and authentication method.

### Data Normalization (Pipeline Configuration)

Snowflake SQL API responses are JSON objects containing both metadata and data arrays. To normalize events for downstream analytics, use an **Eval** function in your pipeline similar to:

```javascript
const results = JSON.parse(_raw).data;
const fields = JSON.parse(_raw).resultSetMetaData.rowType.map(f => f.name.toLowerCase());
let newEvents = [];
for (let row of results) {
  let newEvent = {};
  for (let i = 0; i < fields.length; i++) {
    newEvent[fields[i]] = row[i];
  }
  newEvents.push(newEvent);
}
_raw = JSON.stringify(newEvents);
```
- This logic turns each SQL API result row into a normalized event with key-value fields for column names.

## Usage

Schedule or run the collector to ingest Snowflake audit data. Configure your pipeline and destination to route normalized data to your analytics or SIEM platform as needed.

- **Multiple Collectors:** You may create collectors for different Snowflake audit tables (e.g., QUERY_HISTORY, LOGIN_HISTORY) as required.
- **State Tracking:** Enable Cribl Collector state tracking to avoid duplicate or missing data across runs.

## Troubleshooting

- Make sure your Snowflake network policy allows Cribl’s IP addresses
- Ensure your JWT/OAuth token is not expired
- Review Cribl Stream logs for REST collector errors
- Adjust parameters (timeouts, retries) for large or slow queries as needed

## References

- Cribl Stream REST Collector Docs
- Guide: Using REST/API Collectors
- [Snowflake SQL API documentation (external)]
- Example Cribl pack for Snowflake (available on the Cribl Dispensary)

## Contributing

- Submit pull requests for improvements or fixes
- Open issues for problems or suggestions

## Disclaimer

Be sure to sanitize any sensitive data (tokens, credentials, etc.) before committing configuration files to version control.

## Authors

Kam Amir -- kamilo@cribl.io 
