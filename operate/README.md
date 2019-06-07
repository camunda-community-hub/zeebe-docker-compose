# Zeebe with Operate

This directory contains a profile to run a single Zeebe broker with the embedded gateway, and an instance of Camunda Operate, with required components.

The components that will be started with this profile:

* Zeebe broker
* Camunda Operate
* Elastic Search
* Kibana

## Prerequisites

* [Docker](https://docs.docker.com/compose/install/)

## Preparation

* Clone this repository:

```bash
git clone https://github.com/zeebe-io/zeebe-docker-compose
```

## Start the containers

* Change into this directory:

```bash
cd zeebe-docker-compose/operate
```

* Start the profile:

```bash
docker-compose up
```

## Stop and remove the containers

* Press `Ctrl-C` to stop the containers.

* Destroy the stopped containers:

```bash
docker-compose down
```

## Remove persistent data

The `docker-compose.yml` file specifies persistent volumes for both Zeebe and Camunda Operate. This means that between executions your data is persisted. You may wish to remove all data to start from nothing. To do this, you need to delete the persistent volumes.

To delete all persistent data:

* Stop and remove the containers.

* Run the following command:

```bash
docker volume rm operate_zeebe_data
docker volume rm operate_zeebe_elasticsearch_data
```