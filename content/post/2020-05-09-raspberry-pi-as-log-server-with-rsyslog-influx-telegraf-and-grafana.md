---
title: Log Server with Rsyslog, Influx, Telegraf and Grafana
draft: true
author: mrhenhan
date: '2020-05-09'
slug: raspberry-pi-as-log-server-with-rsyslog-influx-telegraf-and-grafana
categories:
  - Linux
  - Logging
  - Raspberry
tags:
  - Grafana
  - Influx DB
  - Telegraf
  - Rsyslog
  - Raspberry Pi
  - Time Series
subtitle: 'Central Raspberry Pi 4b Log Server with Rsyslog, Influx DB, Telegraf and Grafana'
summary: ''
authors: [mrhenhan]
lastmod: '2020-05-09T23:15:11+02:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---
Outside my professional life as a data scientist and software engineer at [All.In Data](https://www.all-in-data.de/de/), where we primarily focus on [Microsoft Azure](https://azure.microsoft.com) and [Amazon Web Services](https://aws.amazon.com/) cloud development, I am a convinced supporter of providing and hosting the services I use on my own systems. This fact is the reason why the zoo of services and IoT devices I use is constantly growing, eventually giving rise to the need to run a _centralized logging service_ to keep track of generated events at a single place.

As I already use a [Raspberry Pi 4b](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) as an energy efficient (~3W/h over PoE) permanently on [Pi-hole](https://pi-hole.net/), thus it makes sense to use the Pi 4b for hosting the central log server too, especially since the Pi 4b is clearly underutilized serving as local forwarding DNS server only.

Aside of endpoints, multiple infrastructure devices, e.g. a UniFi security gateway, multiple UniFi switches and access points constantly emitting persitable log messages intended for visualization and analysis. Basically,  a log entry represents an observation at a distinct point in time, meaning logs can be viewed as time series which makes using a time series database like [Influx DB](https://www.influxdata.com/) a naturally fit for persisting log entries. Most Linux enabled devices already use [Rsyslog](https://www.rsyslog.com/) as standard system logger with the possibility to configure Rsyslog as a stand-alone service or either as a client or server, enabling Rsyslog using devices to send their logs to a central Rsyslog logging server. For collecting and persisting the received Rsyslog log messages in an Influx DB, [Telegraf](https://www.influxdata.com/time-series-platform/telegraf/) can be used. Finally, [Grafana](https://grafana.com/) will serve the purpose of visualizing log data in form of use case specific dashboards and panels, allowing for easy identification of infrastructure issues and automatically react on configured events.

In the remaining part of the article I show a simple basic configuration of the individual services Rsyslog, Influx DB, Telegraf and Grafana. So, here we go :D


## Installation and Configuration Procedure

- Raspberry Pi
  - Install Influx DB
    - Create User
    - Create Database
  - Install Telegraf
    - Configure for receiving Rsyslog messages
  - Install Rsyslog
    - Configure as logging service for remote systems
    - Configure for sending messages to Telegraf
  - Install Grafana
    - Connect to Influx DB
- Remote Systems
  - Install Rsyslog
  - Configure Rsyslog/systems sending logs to Raspberry Pi
- Grafana
  - Create logging metrics dashboard
  

## Raspberry Pi Configuration

### Influx DB

### Telegraf

### Rsyslog

### Grafana

## Remote System Configuration


## Grafana Logging Metrics
