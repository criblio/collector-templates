# Postman Audit Logs REST Collector

This collector template uses the [Postman API](https://learning.postman.com/docs/developer/postman-api/intro-api/)
to periodically retrieve [Postman Audit Logs](https://learning.postman.com/docs/administration/audit-logs/)
from your Postman team, so you can route them to a SIEM or object storage for
long-term search and retention.

The collector calls `GET https://api.getpostman.com/audit/logs`, which returns
audit events under a top-level `trails` array along with a `nextCursor` value
for pagination.

> **Note:** Audit logs are available on Postman Enterprise plans, and the API
> key you use must be able to read them (see [Generating a Postman API Key](#generating-a-postman-api-key)).
> See the
> [Postman audit logs documentation](https://learning.postman.com/docs/administration/audit-logs/)
> for current plan and permission requirements.

## Requirements

You will need the following values to complete the integration:

| Item | Value |
| ----------- | ----------- |
| Collect URL | `https://api.getpostman.com/audit/logs` (already set in the template) |
| API Key | A Postman API key with Team Admin access — a **Service Account** key is recommended (see below) |

## Generating a Postman API Key

The collector authenticates by sending the API key in the `X-API-Key` request
header.

**Use a Postman [Service Account](https://learning.postman.com/docs/administration/managing-your-team/service-accounts/)
with the Team Admin role**, rather than a personal user account. A service
account decouples the integration from any individual employee, so collection
keeps working when people change roles or leave the team, and the key's access
is scoped to exactly what it needs.

1. As a Team Admin, create (or open) a **Service Account** for your Postman team
   and grant it the **Admin** role on the Team.
2. Generate an API key for that service account at
   **Postman > Settings > API keys** (`https://go.postman.co/settings/me/api-keys`).
3. Copy the key — you will provide it when you import the collector, or store it
   as a Cribl Secret (recommended; see below).

## Providing the API Key

The template ships with the API key as an import **placeholder**, so Cribl
Stream prompts you to enter the key when you import
`collector.json`:

```json
"collectRequestHeaders": [
  {
    "name": "X-API-Key",
    "value": "'<Postman API Key|API key for a Postman Service Account with Team Admin access — create one at https://go.postman.co/settings/me/api-keys>'"
  }
]
```

### Recommended: store the API key as a Cribl Secret

For production deployments, store the key in a Cribl Stream **text secret** and
reference it from the header so the key is never written into the collector
configuration:

1. Navigate to **Manage > Group Settings > Security > Secrets** (or **Settings >
   Security > Secrets** on a single-instance deployment).
2. Click **Add Secret**.
3. Set **Secret type** to **Text**.
4. Give the secret a name — for example, `postman-audit-key`.
5. Paste your Postman API key into the **Value** field, then click **Save**.
6. After importing `collector.json`, edit the `X-API-Key` header value to
   reference the secret instead of the literal key:

   ```json
   "collectRequestHeaders": [
     {
       "name": "X-API-Key",
       "value": "`${C.Secret('postman-audit-key','text').value}`"
     }
   ]
   ```

## Downloading Configuration Files

Download the configuration files from this template directory:

- `breaker.json` — the Event Breaker ruleset
- `collector.json` — the REST Collector

## Import the Event Breaker

The Event Breaker splits the `trails` array into one event per audit record and
sets each event's `_time` from the record's `timestamp` field.

1. Navigate to **Manage > Processing > Knowledge > Event Breaker Rules**.
2. Click **Add Ruleset**.
3. Click **Manage as JSON** at lower left.
4. Click **Import** at top right and select the `breaker.json` file you
   downloaded.
5. Confirm the ruleset is named **Postman Audit Logs** — this is the name the
   collector references in its `breakerRulesets` setting.
6. Click **Save**.

## Import and Configure the REST Collector

From the top nav of a Cribl Stream instance or Group, select **Data > Sources**,
then select **Collectors > REST**. Click **Add Collector** to open the **REST >
New Collector** modal.

1. Click **Configure as JSON** at the top of the window.
2. Select **Import** from the top right and choose the `collector.json` file you
   downloaded.
3. When prompted, enter your Postman API key for the **Postman API Key**
   placeholder. (Alternatively, leave the placeholder, save, then edit the
   `X-API-Key` header to reference a Cribl Secret as described in
   [Recommended: store the API key as a Cribl Secret](#recommended-store-the-api-key-as-a-cribl-secret).)
4. Click **Save**.

## Scheduling and State Tracking

The collector uses a relative time range of **Earliest** `-1d` and **Latest**
`now`, with state tracking enabled. The `since` request parameter is derived from
state, so collection picks up where the last run left off:

- On the **first run** (no saved state), collection starts from **Earliest**.
- On **subsequent runs**, collection starts from `state.latestTime` — the highest
  event `_time` seen so far — through **Latest** (`now`).

The breaker extracts each record's `timestamp` field into `_time`, and the
default state expressions record the highest `_time` in `state.latestTime`.
Because `since` follows state rather than a fixed window, you can run the
collector as frequently as you like without re-collecting overlapping windows.

By default the collector runs every 15 minutes (`cronSchedule: "*/15 * * * *"`)
with `maxConcurrentRuns: 1` and `skippable: true`, so a long-running job is never
overlapped by the next scheduled one. Adjust the `cronSchedule` and time range to
match your latency and retention needs.

## Pagination

The Postman Audit Logs API returns at most `limit` records per response (the
template requests `100`) and includes a `nextCursor` value when more records are
available. The collector is configured for `response_body` pagination on the
`nextCursor` attribute and passes it back as the `cursor` request parameter on
each successive call until no further cursor is returned.

## Timestamps

Each audit record carries a `timestamp` field. The Event Breaker is configured
with `jsonTimeField: "timestamp"`, so Cribl Stream sets `_time` from that value
rather than the collection time.

## Testing

Run a Preview before scheduling:

1. On the **Manage REST Collectors** page, click **Run** beside the collector.
2. In the **Run Collector** modal, select **Mode > Preview**.
3. Populate the **Earliest** time field (for example, `-1d`). Leaving **Latest**
   blank defaults to `now`.
4. Click **Run** to retrieve Preview results and confirm events break correctly
   and timestamps are extracted as expected.

## Sample Data

A sanitized example of the raw `GET /audit/logs` response is provided in
[`samples/postman_audit_logs.ndjson`](samples/postman_audit_logs.ndjson). It is a
real response (10 `trails` records covering several `action` types) with all
identifying data replaced — names, emails, usernames, team name, user/team IDs,
IP addresses, and the Slack workspace ID are fictional, and API key identifiers
were already masked by the API. Paste it into the Event Breaker's sample editor
to validate event breaking and timestamp extraction without live API access.

## Author

Stacy Simmons &lt;sasimmons9629@gmail.com&gt;
