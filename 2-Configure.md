# Configuration

## 1. Prometheus

After Installing, make sure that prometheus and Alert Manager are running, then configure alert manager in `/etc/prometheus.yml` as follows:

```# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
       - localhost:9093 ```

In `/etc/prometheus.yml` , add the following lines for a custom alert.rules file:

`# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  - "/etc/prometheus/alert.rules"`

*You may also refer to the prometheus.yml file in the conf directory of this repo regarding the above configuration.*

## 2 . AlertManager

Next, configure Notification Channels in `/etc/alertmanager.yml` .

* To Set Email notifications, use the following configuration:
    ```global:
  smtp_smarthost: 'localhost:25'
  smtp_from: 'alertmanager@bykea.qa'
  smtp_auth_username: ''
  smtp_auth_password: ''
  smtp_require_tls: false

templates:
- '/etc/alertmanager/template/*.tmpl'

route:
  group_by: [alertname, job]
  receiver: global
```


