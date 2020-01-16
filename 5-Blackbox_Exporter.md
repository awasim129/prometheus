# Blackbox Exporter

Blackbox Exporter is a Prometheus official Exporter tools which is used to probe site Uptime by probing a URL against a Status Code. The Blackbox exporter **should be installed on the Prometheus machine** instead of installing it on agents.

## Installation:

Blackbox Exporter can be installed as a service as well as can be started using Docker Image. To start Blackbox using docker, follow the steps below:

* `docker pull prom/blackbox-exporter`
* `docker run --rm -d -p 9115:9115 --name blackbox_exporter`

*Docker must be installed on Prometheus Server.*

## Configuration:

* Edit `prometheus.yml` file and add the following information to probe urls using Blackbox:

```
scrape_configs:
  - job_name: 'blackbox'
    metrics_path: /probe
    params:
      module: [http_2xx]  # Look for a HTTP 200 response.
    static_configs:
      - targets:
        - http://prometheus.io    # Target to probe with http.
        - https://prometheus.io   # Target to probe with https.
        - http://example.com:8080 # Target to probe with http on port 8080.
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: 127.0.0.1:9115  # The blackbox exporter's real hostname:port.
```
*Replace the target urls with your own URLs*

* Restart Prometheus and Alert Manager.

* On status bar, go to Status > Targets. Both your node exporter and blackbox targets should be visible here.