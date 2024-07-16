# Code42 REST Collector

To create a new Collector for Code42, copy the code in collector.json, then go into Cribl, add a new REST Collector, and click Manage as JSON. Paste the code into the box, replacing all other contents. When you click okay, you will be prompted to fill in username, password and the access key (credentials).

There is no special event breaker required for this collectore.

The Collector defines a field named `source` with value `code42` which you can use to identify events in routing tables, etc.

By default it is scheduled to run every 15 minutes. Change as required.
