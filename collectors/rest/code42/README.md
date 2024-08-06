# Code42 REST Collectors

There are currently 2 collectors available, and 1 EB rule set.

You can find details about file events and query alerts in the [Code42 API docs](https://developer.code42.com/api/#section/User-Guides/Search-file-events)


To create a new Collector for Code42, copy the code in collector.json, then go into Cribl, add a new REST Collector, and click Manage as JSON. Paste the code into the box, replacing all other contents. When you click okay, you will be prompted to fill in username, password and the access key (credentials).

To create the EB, under Processing to Knowledge to Event Breakers, add new, manage as JSON and paste in the code42-eventbreaker.json content. There is one breaker for each collector here, but they are very similar.

The Collector defines a field named `source` with value `code42:fileevent` or `code42:alert` which you can use to identify events in routing tables, etc.

The Collectors are not scheduled by default. Once you've tested and confirmed they're working as expected, a 15 minute cycle is recommended.
