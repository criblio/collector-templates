# O365 Activity REST Collectors

These collector templates let you pull **Office 365 Activity** logs for every available content type **with built-in state tracking**.  
Use them today—full state-tracking support is coming to the native O365 Activity Source soon.

## Supported Content Types

- **`Audit.General`** → `O365_Activity-General.json`
- **`Audit.SharePoint`** → `O365_Activity-SharePoint.json`
- **`Audit.Exchange`** → `O365_Activity-Exchange.json`
- **`Audit.AzureActiveDirectory`** → `O365_Activity-AzureActiveDirectory.json`
- **`DLP.All`** → `O365_Activity-DLP.json`

## Installation (12 easy steps)

1. **Create two global variables**  
   `Processing → Knowledge → Variables`  
   - Name: `o365_tenant_id` → Value: `"your-tenant-id"` (quoted string)  
   - Name: `o365_publisher_id` → Value: `"your-app-id"` (quoted string)

2. Navigate to **Data → Sources → REST Collectors**

3. Click **+ New REST Collector**

4. Switch to the **JSON** tab (top of the window)

5. Click **Import** → choose the desired JSON file (e.g. `O365_Activity-General.json`) → **Save**

6. Open the **Authentication** tab  
   - Set **Client secret parameter** → your O365 client secret

7. Still in **Authentication** → **Extra authentication parameters**  
   - Set `client_id` → your O365 client ID

8. Click **Save**

9. Enable state tracking  
   - Open the **Schedule** tab  
   - Turn on **State Tracking** → **Enable**  
   - Keep default **State update expression** and **State merge expression**  
   - Click **Save**

10. (Optional) **Commit & Deploy**

11. **Repeat steps 3–10** for every content type you want to collect.

12. You’re done—logs will flow with full state tracking!

## Author

Harry Gardner – [hgardner@cribl.io](mailto:hgardner@cribl.io)  
X: [@HarryGardnerIV](https://x.com/HarryGardnerIV)
