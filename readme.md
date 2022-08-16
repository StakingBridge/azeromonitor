# Monitoring your Aleph Zero Validator 

### Monitor your CPU, RAM, Network and Aleph Zero Chain stats

This solution uses Telegraf, Prometheus and Grafana to provide users and node managers a monitoring tool to analyze CPU, RAM, network interfaces and I/O wait along with metrics from Aleph Zero Chain that will be displayed in the public dashboard. Why should you monitor your node using [the public dashboard](https://stafimonitor.stakingbridge.com/)?	
- Control the use of resources in your server.
- It allows you to detect problems, even before they happen.
- Maximizes the security of the Azero network and minimizes the risk of slashing.
- Transparency: allows everyone to see how stable your validator works.

![](https://github.com/StakingBridge/azeromonitor/blob/main/1.png?raw=true)

### 1. INSTALL TELEGRAF IN YOUR NODE SERVER

**UBUNTU 20.04**

```

wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -
source /etc/lsb-release
echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
apt update
apt install telegraf

```

### 2. ADD AZERO NODE AND BASIC CONFIGURATION TO TELEGRAF



Modify the file /etc/telegraf/telegraf.conf and use this 👉[config file](https://github.com/StakingBridge/azeromonitor/blob/main/telegraf.conf).

The file will be such that:

```
# Global Agent Configuration
[agent]
  hostname = "YOUR_NODE_ALIAS" # set this to a name you want to identify your node in the grafana dashboard
  flush_interval = "15s"
  interval = "15s"
# Input Plugins
[[inputs.cpu]]
    percpu = true
    totalcpu = true
    collect_cpu_time = false
    report_active = false
[[inputs.disk]]
    ignore_fs = ["devtmpfs", "devfs"]
[[inputs.io]]
[[inputs.mem]]
[[inputs.net]]
[[inputs.system]]
[[inputs.swap]]
[[inputs.netstat]]
[[inputs.processes]]
[[inputs.kernel]]
[[inputs.diskio]]
[[inputs.prometheus]]
  urls = ["http://localhost:9615"]
# Output Plugin InfluxDB
[[outputs.influxdb]]
  database = "azero"
  urls = [ "https://stats.stakingbridge.com:8086" ] 
  username = "azero"
  password = "azeropassword"

```

### 3. LAUNCH TELEGRAF AND MONITOR YOUR NODE!

- To launch Telegraf: **sudo systemctl start telegraf**
- Visit [https://stats.stakingbridge.com/](https://stats.stakingbridge.com:3000/goto/Up7-oEi4z?orgId=1)

> From top side, use the search engine where it says ¨**Pick Validator**¨ and look for the alias that you have configured in the telegraf.conf file. For example, searching “Stakingbridge_TR-3970X” shows stats for stakingbridge.com validator.


![](https://github.com/StakingBridge/azeromonitor/blob/main/images/GIFZERO.gif?raw=true)

