---
layout: post
title:  "Continuous monitoring"
date:   2021-09-23 00:00:00 +0200
categories: jekyll update
---

## Continuous monitoring

Continuous monitoring is the ability to detect risk, compliance and security issues in an operation environment.

### Monitoring types

There are four main monitoring types:
* Real-time monitoring: CPU statistics, RAM usage, disk usage, I/O operations...
* Infrastructure monitoring: CPU and memory, network, web servers, app servers, DB servers...
* Application monitoring: API success or failure, performance metrics count, API accessibility...
* Network monitoring: Number of packets per second, total bytes transfered...

### Tools

There are a wide variety of continuous monitoring tools. Some of them are:
* Nagios
* ELK Stack
* Datadog
* Splunk
* Grafana
* Ganglia
* Zabbix
* Sensu
* New Relic
* AWS Cloudwatch

## Continuous monitoring with Nagios

Nagios is an open source continuous monitoring tool that allows for system, network and IT infrastructure monitoring.

It is available in two variants:
* Nagios core: Open source.
* Nagios XI: Licensed.

There are some benefits to using Nagios:
* Nagios finds any type of network issue.
* Nagios monitors the infrastructure actively.
* Nagios can facilitate automatic problem resolution.
* Nagios troubleshoots server performance issues.

### Nagios architecture

Nagios uses a client-server architecture, with the server running on a host, and the plugins running in the clients, which are specified for monitoring.

### Nagios plugins

Types:
* Official: What you should tend to use.
* Community: What you can use.
* Custom: What you should build and use if there is nothing official or in the community for what you're trying to do.

## ELK stack

It is an open source, distributed monitoring solution suitable for almost any data source.

It is the acronym for 4 open source projects:
* Elasticsearch: Distributed search and analytics engine that provides data in real time. Highly scalable, it can store complex data structures.
* Logstash: Data processing for Elasticsearch.
* Kibana: Data visualization tool. It provides the GUI for Elasticsearch.
* Beats: This was added after the initial acronym was created. It a lightweight data collector, installed directly on the data source. There various data collectors:
  * Filebeat: Sends local file records.
  * Winlogbeat: Sends Windows event logs.
  * Metricbeat: Sends system performance metrics.

The ELK stack is very efficient, and everyday more and more organizations are starting to use it.