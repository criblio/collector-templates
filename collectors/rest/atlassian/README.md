# Atlassian Audit REST API Collector

This collector template allows you to collect the audit logs from Atlassian Cloud Confluence and Jira instances. 

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
