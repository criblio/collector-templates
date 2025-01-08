# Atlassian Audit REST API Collector

This collector template allows you to collect the audit logs from Atlassian Cloud Confluence and Jira instances. 

# Installation

* Copy the contents of breaker.json*
*  Navigate to Event Breaker Rules under Processing -> Knowledge
* Create a new breaker ruleset
* Edit as JSON and paste the previously copied JSON into place; save
* For each collector JSON file, copy the contents
  1) Navigate to Data -> Sources -> REST Collectors
  2) Add Collector
  3) Configure as JSON tab (at the top of the window)
  4) Paste the JSON from collector into the screen and save
  5) If there are fields for you to fill out, you will be prompted here. When done, hit Replace variables.
* Commit and Deploy

## Configuring

You must replace the following placeholders in the configuration:
* Atlassian Tenant Subdomain - the subdomain of your Atlassian Cloud instance (e.g. https://<instance>.atlassian.net)
* Username - the user with permissions to collect audit logs
* Password - the password for the audit user

Ensure you schedule this job and provide a relevant time range. The recommendation is to use the following:

Earliest: `-10m@m`
Latest: `-5m@m`

### Event Breaker

Import the provided event breaker (Atlassian REST APIs).

## Author
Brendan Dalpe - bdalpe@gmail.com
