# Event Store Exporter

This profile starts the broker with the Event Store exporter [source code](https://github.com/jwulf/zeebe-eventstore-exporter).

[Event Store](https://eventstore.org) is an open-source stream database that supports user-defined projections.

The Event Store exporter batches up events to export and sends them every 300ms (by default) to an Event Store database.

You can access the Event Store web interface at [http://localhost:2113](http://localhost:2113). The credentials to log in are admin / changeit.

## Demo Exporter (Minimal Example)

It also contains a commented out configuration for the demo exporter - one that logs all events to the console.

The demo exporter is the minimal possible exporter. The source code is available [here](https://github.com/jwulf/zeebe-exporter-demo).