# Create an IoT Gateway with InfluxData and Resin IO

### Motivation

In many IoT installations users want to collect all the data for a site, or series of sensors in a central location before shipping to the cloud. This piece of infrastructure is commonly called a Gateway. Gateways can perform a variety of functions but commonly aggregate, store, analyze, and visualize data coming from the sensors. They also ship this data to the cloud. The InfluxData TICK stack ably performs all of these functions, acting as an [IoT Data platform](https://www.influxdata.com/customers/iot-data-platform/).

### Requirements

To follow this walkthrough you are going to need the following:
- Raspberry Pi
- [An InfluxCloud instance](https://cloud.influxdata.com/)
  * Spin up an Uno with your free trial, or use an existing instance.
- [A Resin.io Account](https://docs.resin.io/raspberrypi3/nodejs/getting-started/#let-s-jump-in)
  * The above getting started documentation is for a RaspberryPi 3

Follow the instructions above to connect your RaspberryPi. When the walkthrough has you [deploying code](https://docs.resin.io/raspberrypi3/nodejs/getting-started/#deploying-code) stop.

### Fork Repo

Fork this repo to your own Github account, clone it to your local machine, and add the Resin remote to enable pushing to devices:

```
$ git remote add resin <resin_user>@git.resin.io:<resin_user>/<application_name>.git
```

### Configure

This repo gives you the ability to easily configure the different members of the stack. The configuration files for each product live in the `config` directory. To change the configuration, just edit the files.

#### `telegraf.conf`

This file controls the Telegraf configuration. [Telegraf](https://github.com/influxdata/telegraf) is a plugin-driven, agent based data collection tool. It contains plugins to collect data from many standard sources. In addition, telegraf has a number of utility plugins to help gather data from sources that aren't implemented. It also provides an easy interface for building your own plugin. 

The plugins that are enabled in this installation can be divided into the following groups:
* Managing sensor ingest:
  - This will listen for any data posted to the Gateway by other sensors in the environment.
  - Currently enabled: `http_listener`
* Network monitoring:
  - These plugins help you troubleshoot network issues.
  - Currently enabled: `nstat`, `dns_query`, `ping`
* Host level monitoring: 
  - These plugins give you data about the gateway host itself.
  - Currently enabled: `cpu`, `mem`, `diskio`, `swap`, `system`, `disk`, `kernel`, `processes`
* TICK Stack Monitoring:
  - These plugins give you more data about the TICK Stack installation itself:
  - Currently enabled: `influxdb`,`kapacitor`, `internal`
  
#### `kapacitor.conf`

This file controls the Kapacitor configuration. [Kapacitor](https://github.com/influxdata/kapacitor) is a stream processing tool for processing, monitoring, and alerting on time series data. The configuration you will need to change will be for your metrics shipping to a cloud instance. 

To enable shipping change the configuration in the `[[influxdb]]name="cloudInflux"` section. You will need to provide connection credentials to a running InfluxDB server that is reachable from the device. Spinning up an [Uno instance on InfluxCloud](https://cloud.influxdata.com/cloud#cluster-form) is a great way to give this a try. Change the lines with 🌟 to the proper values for your instance:

```toml
[[influxdb]]
  enabled = true 🌟
  default = false
  name = "cloudInflux"
  urls = ["https://{{cloud-instance}}.influxcloud.net:8086"] 🌟
  username = "myUser" 🌟 
  password = "myPass" 🌟
  timeout = 0
  insecure-skip-verify = false
  startup-timeout = "5m"
  disable-subscriptions = true
  subscription-protocol = "http"
  subscriptions-sync-interval = "1m0s"
  kapacitor-hostname = ""
  http-port = 0
  udp-bind = ""
  udp-buffer = 1000
  udp-read-buffer = 0
  [influxdb.subscriptions]
  [influxdb.excluded-subscriptions]
```

#### `influxdb.conf`

This file controls the InfluxDB configuration. [InfluxDB](https://github.com/influxdata/influxdb) is a scalable datastore for metrics, events, and real-time analytics. You shouldn't need to change any of the configuration in this file. Enabling the `[[graphite]]`,`[[opentsdb]]`, or `[[udp]]` sections is not recomended. If you need to write these formats with this setup please use the [`socket_listener`](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/socket_listener) plugin on telegraf to write that data. This ensures that the metric shipping will operate as expected. 

### Push code!

Once you have configured the repo just run `git push resin master` to deploy your code to your RaspberryPi