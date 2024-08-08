# Code42 REST Collectors

There are currently 3 collectors available, and 1 EB rule set.

You can find details about file events, alerts query, and alerts details in the [Code42 API docs](https://developer.code42.com/api/)

First create the EB: Under Processing to Knowledge to Event Breakers, add new, manage as JSON and paste in the code42-eventbreaker.json content. There is one breaker for each collector here, but they are very similar.

Then create a new Collector for Code42: Copy the code in the *-collector.json file, then go into Cribl, add a new REST Collector, and click Manage as JSON. Paste the code into the box, replacing all other contents. When you click okay, you will be prompted to fill in variables.


The Collector defines a field named `source`, which you can use to identify events in routing tables, etc.:
* for file events: `code42:fileevent`
* for alerts list: `code42:alert`
* for alerts details: `code42:alert_detail`

The Collectors are not scheduled by default. Once you've tested and confirmed they're working as expected, a 15 minute cycle is recommended.

Some collectors will require a tenant ID. You need to retreieve this from the API after getting a bear token. HTTP conversations are below to help with this step.

Authentication step to get a token:

```
POST https://api.us.code42.com/v1/oauth
Content-type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=CLIENT_ID_HERE&client_secret=CLIENT_SECRET_HERE
```

In curl terms, that would be something like this:
```
curl https://api.us.code42.com/v1/oauth \
   -H 'Content-type: application/x-www-form-urlencoded' \
   -d 'grant_type=client_credentials&client_id=CLIENT_ID_HERE&client_secret=CLIENT_SECRET_HERE'
```

The bearer token will be in the returned JSON. Once you have the bearer token, you can make this request to get the tenant ID. As far as I can tell, this will be static.

```
GET https://api.us.code42.com/v1/customer
Authorization: Bearer tokenhere
```

And the curl for that step would look like this:

```
curl https://api.us.code42.com/v1/customer \
   -H 'Authorization: Bearer tokenhere'
```

The tenant ID will be in the returned JSON.
