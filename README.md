Docker Swarm Monitoring
===

Docker Stack compose file to deploy dynamic monitoring for Docker Swarm. This stack deploys the following services:

* `cAdvisor`: Deployed to all nodes to collect metrics from the system and containers running in it.
* `influxDB`: Time-series database to store the metrics.
* `Grafana`: Graphing tool to setup dashboards and alerts.

Run Instructions
---
Setup a Docker Swarm with the Docker Swarm Mode. From the swarm manager, deploy the stack with the command,
```
docker stack deploy -c docker-stack.yml monitor
```

A database named `cadvisor` is needed in influxDB.
```
docker exec `docker ps | grep -i influx | awk '{print $1}'` influx -execute 'CREATE DATABASE cadvisor' 
```

Open Grafana in your browser with the following command:
```
open http://`docker-machine ip manager`
```

In Grafana, create a new `InfluxDB` data source, with Url `http://influx:8086` and database `cadvisor`. Finally import new dashboard with `dashboard.json` file.