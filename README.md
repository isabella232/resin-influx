# resin-influx
============

A basic example of running the [InfluxData](https://influxdata.com/) TICK Stack via [resin.io](https://resin.io).

This Dockerfile spins up the following processes:

- [Telegraf](https://github.com/influxdata/telegraf) running the following plugins for data gathering:
    * [cpu](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/CPU_README.md)
    * [disk](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/DISK_README.md)
    * [mem](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/MEM_README.md)
    * [system](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/SYSTEM_README.md)
    * [diskio](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/)
    * [kernel](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/KERNEL_README.md)
    * [swap](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/)
    * [processes](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/PROCESSES_README.md)
    * [dns_query](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/dns_query)
    * [nstat](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/system/NSTAT_README.md)
    * [ping](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/ping)
- [InfluxDB](https://github.com/influxdata/influxdb)
- [Kapacitor](https://github.com/influxdata/kapacitor)
- [Chronograf](https://github.com/influxdata/chronograf)

