# Configuring Alerts in Alert Manager

During installation, we created a file `alert.rules` in the `/etc/prometheus` directory. Alerts that we want to appear should be configured in this file.

Alerts using PromQL queries as well as comparisons. Some sample configurations for `alert.rules` file are mentioned below.

**Whenever the alerts are fired, notifications will be sent to the notification channel defined in `alertmanager.yml` file**
*Refer to the default alert.rules file as well in the conf directory. Also, restart Prometheus after configuration update.*

### Alert Rules for System Metrics:

```
- name: production-servers
  rules:
  - alert: cpuUsage
    expr: 100 - (avg by (instance) (irate(node_cpu_seconds_total{job='node' ,mode="idle"}[5m])) * 100) > 80
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: "CPU usage is greater than 80%. Please check"
  - alert: memUsage
    expr: 100 - ((node_memory_MemAvailable_bytes{job="node"} * 100) / node_memory_MemTotal_bytes{job="node"}) > 80
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: "MEMORY Usage is greater than 80%. Please check"
  - alert: diskUsage
    expr: 100 - ((node_filesystem_avail_bytes{job="node",mountpoint="/",fstype!="rootfs"} * 100) / node_filesystem_size_bytes{job="node",mountpoint="/",fstype!="rootfs"}) > 80
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: "Disk Usage is greater than 80%. Please check"
  - alert: uptime
    expr: up{job="node"} != 1
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: "instance { $labels.instance }} is down. Please check"
```

### Alert Rules for BlackBox:

```
- name: alert.rules
  rules:
  - alert: EndpointDown
    expr: probe_success == 0
    for: 60s
    labels:
      severity: "critical"
    annotations:
      summary: "Endpoint {{ $labels.instance }} down"
  - alert: PM2ProcessRestart
    expr: pm2_restarts > 0
    for: 10s
    labels:
      severity: "critical"
    annotations:
      summary: "PM2 process restarted for {{ $labels.instance }} "
```