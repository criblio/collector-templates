# O365 Activity REST Collectors

These collector templates enable collection of **Microsoft Office 365 Management Activity API** logs for all supported content types — **with full state tracking** and **proper handling of Microsoft’s built-in ingestion delays**.

Microsoft does **not** provide this data in real time.
Events typically appear in the API **60–90 minutes** (sometimes longer) after they occur in Office 365.
These updated templates automatically account for that lag so you never miss data — even though customers often expect near-real-time visibility.

**All templates in this directory have been enhanced** with smarter `startTime`/`endTime` logic that prevents gaps and overlap.

## Why the new global variables exist

| Variable                      | Type   | Default | Purpose |
|-------------------------------|--------|---------|---------------------------------------------------------------------|
| `o365_ingestion_lag_min`      | Number | `90`    | How many minutes to “look back” to compensate for Microsoft’s data availability delay. 90 minutes covers >99% of events. |
| `o365_polling_interval_min`   | Number | `5`     | How often the collector runs (matches the default `*/5 * * * *` cron). Used on first run to set a safe initial time window. |

These variables make the collectors robust across all tenants and content types — no more missed events due to premature polling.

## Supported Content Types

- **`Audit.General`** → `O365_Activity-General.json`
- **`Audit.SharePoint`** → `O365_Activity-SharePoint.json`
- **`Audit.Exchange`** → `O365_Activity-Exchange.json`
- **`Audit.AzureActiveDirectory`** → `O365_Activity-AzureActiveDirectory.json`
- **`DLP.All`** → `O365_Activity-DLP.json` *(fully updated with lag-aware time windows)*

## Installation

### 1. Create global variables
`Processing → Knowledge → Variables`

| Name                          | Type   | Example / Default Value                  | Notes                                  |
|-------------------------------|--------|------------------------------------------|----------------------------------------|
| `o365_tenant_id`              | String | `"d6e4f0c1-..."`                         | Quote the value                        |
| `o365_publisher_id`           | String | `"a1b2c3d4-..."` (your App/Client ID)    | Quote the value                        |
| `o365_ingestion_lag_min`      | Number | `90`                                     | Adjust higher if your tenant is slower |
| `o365_polling_interval_min`   | Number | `5`                                      | Should match your cron schedule        |

### 2–11. Add each collector
2. Navigate to **Data → Sources → REST Collectors**
3. Click **+ New REST Collector**
4. Switch to the **JSON** tab
5. Click **Import** → select the desired `.json` file → **Save**
6. Authentication tab → set **Client secret parameter** to your app’s client secret
7. Authentication tab → **Extra authentication parameters** → set `client_id` to your app’s client ID
8. Save
9. Schedule tab → **Enable Schedule and State Tracking** (keep default expressions)
10. Save
11. (Optional) Commit & Deploy
12. Repeat for each content type you need

You’re all set — the collectors will now safely handle Microsoft’s 60–90 minute ingestion delay with zero gaps or duplicates.

## Author

Harry Gardner – [hgardner@cribl.io](mailto:hgardner@cribl.io)
