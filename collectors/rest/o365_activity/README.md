# O365 Activity REST Collectors

The collector templates in this directory allow you to collect logs from the Office 365 Activity for all avaiable content types with state tracking support. Use these collectors to achieve state tracking until this feature is added to the Office 365 Activity Source.

The supported o365 Activity content types and the collector file for each are:

Audit.General - O365_Activity-General.json
Audit.SharePoint - O365_Activity-SharePoint.json
Audit.Exchange - O365_Activity-Exchange.json
Audit.AzureActiveDirectory - O365_Activity-AzureActiveDirectory.json
DLP.All - O365_Activity-DLP.json


# Installation

1) Add global variable (Processing -> Knowledge -> Variables) of type string named o365_tenant_id with your o365 Tenant ID. Make sure to quote the variable's value.
2) Add global variable (Processing -> Knowledge -> Variables) of type string named o365_publisher_id with your o365 Publisher ID (a.k.a. App ID). Make sure to quote the variable's value.
3) Navigate to Data -> Sources -> REST Collectors
4) Create a new REST Collector
5) Configure as JSON tab (at the top of the window)
6) Click the import link and select the collector json file of the collector to add (i.e. O365_Activity-General.json) and save.
7) Under the REST Collector's authentication tab update the "Client secret parameter" with your o365 account's client secret.
8) Under the REST Collector's authentication tab update the client_id parameter value under "Extra authentication parameters" with your o365 account's client ID. 
9) Save
10) To enable state tracking for the collector, schedule the collector and click Enable under State Tracking. Use default values for the "State update expression" and "State merge expression" and select Save.
11) Commit and Deploy (optional)
12) Repeat steps 3-11 for each content type you'd like to collect data for.

## Author
Harry Gardner - hgardner@cribl.io
