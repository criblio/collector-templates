# Cribl Billing Credits REST Collector

This Cribl REST Collector is configured to retrieve billing credits data from the Cribl Cloud API:  
`https://api.cribl.cloud/v4/organizations/<orgID>/billing/credits`

## Overview

The collector provides a reliable, secure, and automated way to fetch billing credits information from the Cribl Cloud platform. This data can then be used to:

- Monitor usage trends  
- Track licensing and credit consumption  
- Integrate billing metrics into broader observability or cost-management workflows  

## Why Use This Collector?

Not everyone has direct access to FinOps tools or billing dashboards. In some organizations, responsibility for monitoring license consumption may sit with another team or platform. With this REST Collector, you can independently pull credit consumption data directly from Cribl’s API and make it available in your own environment.  

## Important Note

This collector leverages an **internal API endpoint** that is subject to change. As of now, this endpoint is undocumented, so use with caution and validate against your organization’s needs.  

### Sample Data 
API response example can be found at ```sample_payload_billing_credits.log```

### Authors

Andrew Hendrix - ahendrix@criblservices.io 
Brandon Swanson - Brandon.Swanson@blackbaud.com
