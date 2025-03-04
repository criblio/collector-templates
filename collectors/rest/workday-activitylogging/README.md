# Workday REST Collector

This collector template allows you to collect logs from Workday.

## Configuring Workday

1. Create an Integration System User
  - Access the Create Integration System User task and provide the following parameters:
    - User Name: cribl_ISU.
    - New Password and New Password Verify: Enter the password.
    - Do Not Allow UI Sessions: Check the box.
    - Session Timeout Minutes: 0 (Disable session expiration).
  - Click OK.
    - Exempt the created user from the password expiration rule.
    - Access Maintain Password Rules task.
    - Add the users to System Users exempt from password expiration.
2. Create a Security Group
  - To create a security group, access the Create Security Group task and provide the following parameters:
    - Type of Tenanted Security Group. Integration System Security Group (Unconstrained)
    - Name. Cribl Client Security Group.
  - Click OK.
  - In the Edit Integration System Security Group (Unconstrained) window provide the following parameters:
    - Integration System Users. cribl_ISU.
    - Comment (Optional). Provide a short description.
  - Click OK.
  - To attach the security group to a domain, access the View Domain task for the domain System Auditing.
  - Select Domain > Edit Security Policy Permissions from the System Auditing related Actions menu.
  - Add the Cribl Client Security Group you created to both the tables as below:
    - Report/Task Permissions table. View access.
    - Integration Permissions table. Get access.
  - Click OK.
  - To apply policy changes, access the Activate Pending Security Policy Changes task and activate the changes you made.
  - Click OK.
3. Register the API Client
  - To register the API client, access the Register API Client for Integrations task, and provide the following parameters:
    - Client Name. Cribl Workday Collector
    - Non-Expiring Refresh Tokens. Yes.
    - Scope. System.
  - Click OK.
  - Copy the Client Secret and Client ID before you navigate away from the page and store it securely. If you lose the Client Secret, you can generate a new one using the Generate New API Client Secret task.
  - Click Done.
  - To generate a refresh token, access the View API Clients task and copy the below two parameters from the top of the page:
    - Workday REST API Endpoint. The endpoint to use access to the resources in your Tenant.
    - Token Endpoint. The endpoint used to exchange an authorization code for a token (if you configure authorization code grant).
  - Go to the API Clients for Integrations tab, hover on the "Cribl Workday Collector API" client, and click on the three-dot kebab action buttons.
  - In the new pop up window, click API Client > Manage Refresh Token for Integrations.
  - In the Manage Refresh Token for Integrations window, select "cribl_ISU" in the Workday Account field and click OK.
  - In the newly opened window, select Generate New Refresh Token checkbox and click OK.
  - Copy the value of the Refresh Token column from the opened window and click Done.


### Configuring Collector:
You must replace the following placeholders in the configuration:

`<Customer Directory>` in the collect and login urls
`<client_id>` in the authentication params
`<refresh_token>` in the authentication params
`<client_secret>` in the authentication params


Ensure you schedule this job and provide a relevant time range. The recommendation is to use the following:

cron schedule: `3/5 * * * *`
Earliest: `-6m@m`
Latest: `-1m@m`

### Event Breaker

Import the provided event breaker and configure the REST Collector to use the `Workday-API` event breaker rule set.

## Author
Brian Rampley - brampley@cribl.io