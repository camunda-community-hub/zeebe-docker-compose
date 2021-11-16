[![Community Extension](https://img.shields.io/badge/Community%20Extension-An%20open%20source%20community%20maintained%20project-FF4700)](https://github.com/camunda-community-hub/community) [![Lifecycle: Proof of Concept](https://img.shields.io/badge/Lifecycle-Proof%20of%20Concept-blueviolet)](https://github.com/Camunda-Community-Hub/community/blob/main/extension-lifecycle.md#proof-of-concept-)

# DEPRECATED!

**Important note:** The docker compose configuration files here are deprecated. Please use **the default docker compose file provided in [the get started repository (https://github.com/camunda-cloud/camunda-cloud-get-started/blob/master/docker-compose.yaml)](https://github.com/camunda-cloud/camunda-cloud-get-started/blob/master/docker-compose.yaml)** instead.



# Zeebe + Operate in Docker

This repository contains configuration files to setup an environment to
develop with [Zeebe].

The configurations manage the following Zeebe components:

- [Zeebe] is a workflow engine for micro-services orchestration.

- [Operate](https://zeebe.io/blog/2019/04/announcing-operate-visibility-and-problem-solving/) is an operations tool for monitoring and troubleshooting live workflow instances in Zeebe.

- [Simple Monitor](https://github.com/zeebe-io/zeebe-simple-monitor) is a **community maintained** monitoring tool for development purpose. This should **not** be used in production as it has a performance impact on the broker.

For more information on using Zeebe and Operate, consult the Quickstart Guide in the [Zeebe docs](https://docs.zeebe.io/introduction/quickstart.html).

The `docker-compose.yml` files in this repository can be used to start a single Zeebe broker; optionally with Simple Monitor, or with Operate, along with the Elasticsearch and Kibana containers that Operate needs.

## Versions

* Zeebe 1.1.0
* Operate 1.1.0
* Simple Monitor 2.0.0
* ZeeQS latest
* Zeebe Hazelcast exporter 1.0.0

# Profiles

* [`broker-only`](broker-only/docker-compose.yml) - a single node Zeebe broker
* [`cluster`](cluster/docker-compose.yml) - a three-node cluster configuration
* [`operate`](operate/docker-compose.yml) - a single node Zeebe broker with Operate
* [`operate-simple-monitor`](operate-simple-monitor/docker-compose.yml) - a single node Zeebe broker with Operate and Simple Monitor
* [`simple-monitor`](simple-monitor/docker-compose.yml) -  a single node Zeebe broker with Simple Monitor
* [`standalone-gateway`](standalone-gateway/docker-compose.yml) - a three-node cluster with a standalone gateway
* [`zeeqs`](zeeqs/docker-compose.yml) - a single broker with the ZeeQS query API

# Utilities

* [`bin/zbctl*`](bin) - cli binary to interact with the broker. **Note**: use the `--insecure` flag, as these docker-compose configurations do not have TLS enabled.
* [`bpmn/diagram_1.bpmn`](bpmn) - example diagram to deploy to the broker

## Services / Ports

The containers expose the following services:

- Zeebe broker - port 26500
- Operate - web interface http://localhost:8080 (login: demo/demo)
- ElasticSearch - port http://localhost:9200
- Kibana - port http://localhost:5601
- Simple Monitor - web interface http://localhost:8082

## Prerequisites

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Recommended

To visually inspect and manage running containers and persistent volumes, you can use [Portainer](https://portainer.io).


## Usage

Clone this repository to your local machine:

```bash
git clone https://github.com/zeebe-io/zeebe-docker-compose
```

## Start the Containers in the Foreground

Running the containers in the foreground will tail the output from each of the containers in your console, allowing you to inspect it.

Run the following command in the directory of the profile that you want to start:

```
# change to directory of the profile to start, i.e
# cd operate/
docker-compose up
```

## Stop Containers Running in the Foreground

Closing the terminal (including terminating an ssh connection) or hitting Ctrl-C will stop the containers.

To remove the stopped containers, run the following the command in the directory of the profile that you started:

```bash
docker-compose down
```

## Run the Containers in the Background (Daemon mode)

To start the containers in the background, use the `-d` flag:

```bash
docker-compose up -d
```

## Stop Containers Running in the Background

Run the following command in this directory:

```bash
docker-compose down
```

This will stop the containers and remove them, but will keep the persistent
data folders of the containers. Therefore if you recreate the containers they
will startup with your existing data volumes.

In case you want to clean also the persistent data use the following command
instead:

```bash
docker-compose down -v
```

## Deploy BPMN Files

The `zbctl` binary is included to allow you to interact with the running broker. The binary is named:

| Operating System |   zbctl binary   |
|:----------------:|:----------------:|
|       Linux      | bin/zbctl        |
|       OS X       | bin/zbctl.darwin |
|      Windows     | bin/zbctl.exe    |


```bash
cd zeebe-docker-compose
bin/zbctl deploy ../path/to/your-file.bpmn
```

## Removing Persistent Data

The Operate profiles create persistent volumes. Sometimes you want to flush the data from previous starts. To do this you need to delete the `zeebe_data` and `zeebe_elasticsearch_data` volumes. They are prefixed by the profile name. You can use [Portainer](https://portainer.io) to do this, or using the command line:

### List Persistent Volumes

```
docker volume ls
```

### Delete Persistent Volumes

Stop the running containers first using `docker-compose down` in the directory of the profile you started. Then:

```
# Example for the operate profile
docker volume rm operate_zeebe_data
docker volume rm operate_zeebe_elasticsearch_data
```

## Running with Simple Monitor

One thing that Operate doesn't have is inspection of messages. This can be useful when developing and debugging.

The `operate-simple-monitor` folder contains a docker-compose file that will start Operate _and_ Simple Monitor. Simple Monitor will be running on [http://localhost:8082](http://localhost:8082).

## Error messages during Simple Monitor startup

During the startup of Simple Monitor, you may see error messages in the logs. This is caused by a race condition where the Simple Monitor starts before the exporter has created the database tables that it needs to run. You can ignore these error messages and the container will automatically restart until the needed database tables are created.

## Error messages during Operate startup

During the startup of Operate, you may see error messages in the logs regarding failed connection attempts to ElasticSearch. This is caused by a race condition where Operate starts before ElasticSearch has completed its bootstrap process. Restarting the operate container should resolve the issue.
```
# Example for the operate-simple-monitor profile
docker restart operate
```



## Issues Running on Windows

Windows can have issues mounting files into Linux containers, especially if you run these configurations from a location outside your home directory.

You may see messages when starting, containing error messages similar to this:

```
\\\"/mnt/sda1/var/lib/docker/overlay2/039...7/merged/usr/local/zeebe/conf/application.yaml\\\"
caused \\\"not a directory\\\"\"": unknown:
Are you trying to mount a directory onto a file (or vice-versa)?
Check if the specified host path exists and is the expected type'
```

See [this post in the Zeebe forum](https://forum.zeebe.io/t/docker-compose-operate-error/479/11) for a solution.


[Zeebe]: https://zeebe.io
