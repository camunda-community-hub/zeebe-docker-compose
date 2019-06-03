# Event Store Exporter

This profile starts the broker with the Event Store exporter [source code](https://github.com/jwulf/zeebe-eventstore-exporter/tree/push-worker).

[Event Store](https://eventstore.org) is an open-source stream database that supports user-defined projections.

The Event Store exporter batches up events to export and sends them every 300ms (by default) to an Event Store database.

You can access the Event Store web interface at [http://localhost:2113](http://localhost:2113). The credentials to log in are admin / changeit.

## Push Worker

This branch contains an exporter that exports only job activation, job time out, and job failure events.

Start the broker and Event Store:

```
docker-compose up
```

Now deploy the test workflow:

```
./bin/zbctl.darwin deploy ./bpmn/test-workflow.bpmn
```

And start a workflow instance:

```
./bin/zbctl.darwin create instance test-workflow
```

Now open the [Event Store database web interface](http://l:2113/web/index.html#/streams/zeebe), and examine the exported JSON records. They can be used to invoke the task worker for the appropriate task worker. See the [Zeebe and Fn Project Integrated](https://zeebe.io/blog/2019/04/zeebe-and-fn-project-integrated-a-proof-of-concept/) blog post for a description of how this architecture can be constructed.

