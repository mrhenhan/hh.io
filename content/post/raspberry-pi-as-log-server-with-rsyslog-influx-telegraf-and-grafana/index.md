---
title: Log Server with Rsyslog, Influx, Telegraf and Grafana
draft: false
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

In the remaining part of the article a simple basic configuration of the individual services Rsyslog, Influx DB, Telegraf and Grafana will be presented. So, here we go.


## Installation and Configuration Procedure

{{% alert note %}}
It is assumed that you have installed [Raspbian Buster or Raspbian Buster Lite](https://www.raspberrypi.org/downloads/raspbian/) on your Raspberry Pi.
{{% /alert %}}

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

Before we install the necessary _armfh_ packages with the help of _wget_ and _dpkg_, we will update the package repository and upgrade Raspbian Buster together with the currently installed software.

{{< icon name="terminal" pack="fas" >}}

```bash
apt update && apt upgrade
```

Now, that our Raspberry Pi is up-to-date, we begin with installing the required software.

{{% alert warning %}}
The descibed setup is intended for local use only as *it is insecure*. If you intend to expose described services remotely, you will have to _adapt authentification and privileges according to your intended use case!_ 
{{% /alert %}}

### InfluxDB

Initially we will install the time series database InfluxDB. At the time of writing this article, the latest stable release was 1.8.0. A list of releases can be found at [InfluxDB releases](https://github.com/influxdata/influxdb/releases). The following commands will download, install, enable, and start InfluxDB 1.8.0 as a system service...

{{< icon name="terminal" pack="fas" >}}

```bash
wget https://dl.influxdata.com/influxdb/releases/influxdb_1.8.0_armhf.deb
dpkg -i influxdb_1.8.0_armfh.deb
systemctl enable influxdb
systemctl start influxdb
```

Afterwards, an InfluxDB user with administration privileges and a database can be created via...

{{< icon name="terminal" pack="fas" >}}

```bash
influx
CREATE USER <<yourusername>> WITH PASSWORD <<yourpassword>> WITH ALL PRIVILEGES
CREATE DATABASE <<yourdatabasename>>
exit
```

Finally, we will bind InfluxDB to a HTTP port and enable _authenticated_ HTTP database connections in _/etc/influxdb/influxdb.conf_. Afterwards we need to restart the influxdb systemd service.

{{< icon name="terminal" pack="fas" >}} **/etc/influxdb/influxdb.conf**

```bash
[http]
  enabled = true
  bind-address: ":8086"
  auth-enabled = true
  log-enabled = true
```

Save and exit and restart the influxdb systemd service with

{{< icon name="terminal" pack="fas" >}}

```bash
systemctl restart influxdb
```

Now, that we have a running influxdb and a database user with sufficient priviliges to access our logging database, we will configure Telegraf and Rsyslog to aggregate log messages from our remote systems.

### Telegraf

Telegraf will be used for collecting log messages collected and aggregated by Rsyslog and send them to InfluxDB for persisting. As a plugin-driven server agent Telegraf is capable of interfacing with various kind databases, systems, and IoT sensors. As of the time of writing, the [current version of Telegraf is _v1.14.2_](https://portal.influxdata.com/downloads/).

{{< icon name="terminal" pack="fas" >}}

```bash
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.14.2-1_armhf.deb
dpkg -i telegraf_1.14.2-1_armhf.deb
```

As with InfluxDB before, we enable and start Telegraf as a systemd service.

{{< icon name="terminal" pack="fas" >}}

```bash
systemctl enable telegraf
systemctl start telegraf
```

Before we are going to wire all services, we need to install Rsyslog and configure it for receiving logging messages from remote systems.

### Rsyslog Server

[Rsyslog](https://www.rsyslog.com/), the **r**ocket-fast **sys**tem for **log** processing is a high-performance system and kernel syslogd of modular design and is the standard syslogd on many Linux distributions already. Sadly on Raspbian we have to install and enable it separately. For this we install, enable and start via ...

{{< icon name="terminal" pack="fas" >}}

```bash
apt install rsyslog rsyslog-relp
systemctl enable rsyslog
systemctl start rsyslog
```

Now that we have install Rsyslog, we need to configure it for receiving remote log messages. The following configuration file resembles my rsyslog configuration, using port *514/UDP* where Rsyslog allows log messages from localhost and 3 different networks.

{{< icon name="terminal" pack="fas" >}} **/etc/rsyslog.conf**

```bash
# /etc/rsyslog.conf configuration file for rsyslog
# For more information install rsyslog-doc and see
# /usr/share/doc/rsyslog-doc/html/configuration/index.html

#################
#### MODULES ####
#################

module(load="imuxsock") # provides support for local system logging
module(load="imklog")   # provides kernel logging support
#module(load="immark")  # provides --MARK-- message capability

# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")

# provides TCP syslog reception
#module(load="imtcp")
#input(type="imtcp" port="514")

###########################
#### GLOBAL DIRECTIVES ####
###########################

# Use traditional timestamp format.
# To enable high precision timestamps, comment out the following line.
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat

# Set the default permissions for all log files.
$FileOwner root
$FileGroup adm
$FileCreateMode 0640
$DirCreateMode 0755
$Umask 0022

# Where to place spool and state files
$WorkDirectory /var/spool/rsyslog

# Include all config files in /etc/rsyslog.d/
$IncludeConfig /etc/rsyslog.d/*.conf

###############
#### RULES ####
###############

# First some standard log files.  Log by facility.
auth,authpriv.*			/var/log/auth.log
*.*;auth,authpriv.none		-/var/log/syslog
#cron.*				/var/log/cron.log
daemon.*			-/var/log/daemon.log
kern.*				-/var/log/kern.log
lpr.*				-/var/log/lpr.log
mail.*				-/var/log/mail.log
user.*				-/var/log/user.log

# Logging for the mail system.  Split it up so that
# it is easy to write scripts to parse these files.
mail.info			-/var/log/mail.info
mail.warn			-/var/log/mail.warn
mail.err			/var/log/mail.err

# Some "catch-all" log files.
*.=debug;\
	auth,authpriv.none;\
	news.none;mail.none	-/var/log/debug
*.=info;*.=notice;*.=warn;\
	auth,authpriv.none;\
	cron,daemon.none;\
	mail,news.none		-/var/log/messages

# Emergencies are sent to everybody logged in.
# *.emerg				:omusrmsg:*

# Allow Connections from
$AllowSender UDP, 127.0.0.1, xxx.xxx.1.0/24, xxx.xxx.2.0/24, xxx.xxx.10.0/24

# Log Message Template
$template Incoming-logs,"/var/log/%HOSTNAME%/%PROGRAMNAME%.log"

# for starters, log everything
*.* ?Incoming-logs
```

Additionally we need to create the file _50-telegraf.conf_ in _/etc/rsyslog.d_ for providing Rsyslog data to Telegraf.

{{< icon name="terminal" pack="fas" >}} **/etc/rsyslog.d/50-telegraf.conf**

```bash
$ActionQueueType LinkedList # use asynchronous processing
$ActionQueueFileName srvrfwd # set file name, also enables disk mode
$ActionResumeRetryCount -1 # infinite retries on insert failure
$ActionQueueSaveOnShutdown on # save in-memory data if rsyslog shuts down

# forward over tcp with octet framing according to RFC 5425
# *.* @@(o)127.0.0.1:6514;RSYSLOG_SyslogProtocol23Format

# uncomment to use udp according to RFC 5424
*.* @127.0.0.1:6514;RSYSLOG_SyslogProtocol23Format
```

The systemd rsyslog service has to be restarted now to adopt the new configuration.

{{< icon name="terminal" pack="fas" >}}

```bash
systemctl restart rsyslog
```

Rsyslog and Telegraph offer a wealth of configuration and formatting options and it is recommended that you consult their documentation for more advanced configuration options.

### Rsyslog Client

Rsyslog enabled clients need to know where to send their log messages and how they should operate if the central logging server is not available. Install Rsyslog in the same way as before and use a configuration similar to the following. 

{{< icon name="terminal" pack="fas" >}} **/etc/rsyslog.conf**

```bash
# Minimal config

$ModLoad imuxsock # provides support for local system logging
$ModLoad imklog   # provides kernel logging support
$ActionFileDefaultTemplate RSYSLOG_TraditionalFileFormat
$FileOwner root
$FileGroup root
$FileCreateMode 0640
$DirCreateMode 0755
$Umask 0022
$WorkDirectory /var/spool/rsyslog
$IncludeConfig /etc/rsyslog.d/*.conf

auth,authpriv.*                 /var/log/auth.log
*.*;auth,authpriv.none          -/var/log/syslog
cron.*                          /var/log/cron.log
daemon.*                        -/var/log/daemon.log
kern.*                          -/var/log/kern.log
lpr.*                           -/var/log/lpr.log
mail.*                          -/var/log/mail.log
user.*                          -/var/log/user.log

mail.info                       -/var/log/mail.info
mail.warn                       -/var/log/mail.warn
mail.err                        /var/log/mail.err

news.crit                       /var/log/news/news.crit
news.err                        /var/log/news/news.err
news.notice                     -/var/log/news/news.notice

*.=debug;\
        auth,authpriv.none;\
        news.none;mail.none     -/var/log/debug
*.=info;*.=notice;*.=warn;\
        auth,authpriv.none;\
        cron,daemon.none;\
        mail,news.none          -/var/log/messages

# Enable sending logs via udp port 514 to XXX.XXX.X.X
*.* @XXX.XXX.X.X:514

## Set disk queue when XXX.XXX.X.X is down
$ActionQueueFileName queue
$ActionQueueMaxDiskSpace 1g
$ActionQueueSaveOnShutdown on
$ActionQueueType LinkedList
$ActionResumeRetryCount -1
```

This configuration enables Rsyslog to send log messages to XXX.XXX.X.X via udp port 514 and to cache 1GB of log messages in case the Rsyslog server is down. Configure each client that should send Rsyslog messages to the Rsyslog server accordingly.


### Telegraf Output Plugins

Telegraf needs to know from where to receive input measurements and where to put them. For this we need to add a syslog input and an InfluxDB output plugin in _/etc/telegraf/telegraf.conf_.

{{< icon name="terminal" pack="fas" >}} **/etc/telegraf/telegraf.conf**

```bash
[[inputs.syslog]]
  server = "udp://:6514"
  
[[outputs.influxdb]]
  urls = ["http://127.0.0.1:8086"]
  database = <<yourdatabasename>>
  username = <<yourusername>>
  password = <<yourpassword>>
```

### Grafana
Grafana describes itself as open observability platform and will be used for our logging dashboards. Luckily Grafana recently provides an _armfh_ package for installation on _arm_ systems like a Raspberry Pi.

[The current version is _6.7.3_](https://grafana.com/grafana/download?platform=arm) and we need to install the _ARMv6_ and additionally _libfontconfig1_.

{{< icon name="terminal" pack="fas" >}}

```bash
apt install libfontconfig1
wget https://dl.grafana.com/oss/release/grafana-rpi_6.7.3_armhf.deb
dpkg -i grafana-rpi_6.7.3_armhf.deb
```

And, as previously, enable and start the Grafana systemd service.

{{< icon name="terminal" pack="fas" >}}

```bash
systemctl enable grafana-server
systemctl start grafana-server
```

Grafana should now be available via **http://<Your-Raspberry_Pi's-IP>:3000** with the default credentials _admin/admin_.

Finally, we are ready to add the InfluxDB data source to Grafana with the purpose to receive and visualize log messages coming from our Rsyslog enabled network hosts.

Now login to Grafana and add your data source in section **Configuration/Data Scources**, choose **InfluxDB**, set your credentials and select _HTTP Method = POST_. Optionally set the data source as your default source. Finally click _Save & Test_ at the bottom of the page. 

{{< figure src="./grafana-add-datasource.png" title="Grafana: Add InfluxDB data source" lightbox="true" >}}


### Log Table in Grafana

If all the former configurations were successful till now, we are ready to configure the first logging dashboard showing the aggregated system log stream. Select **Create/Dashboard** and subsequently **New Panel/Add Query**.

Then set a query according to the following screenshot and save the dashboard.

{{< figure src="./grafana-add-query.png" title="Grafana: Add InfluxDB data source" lightbox="true" >}}

Now you should have an output similarily to the following screenshot.

{{< figure src="./grafana-log-stream.png" title="Grafana: Add InfluxDB data source" lightbox="true" >}}

**Congratulations** - You have sucessfully configured a central log server using Rsyslog, the time series database InfluxDB, Telegraf and Grafana as visualizer. As next step you should consider securing your installation and to create more elaborate dashboards and warning triggers that warn you automatically in case of certain events or even perform actions automatically.

Have fun!


