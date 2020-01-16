# Configure Agent Nodes

Prometheus pulls System Metrics from Node Exporter. A Node Exporter is an HTTP based service that displays System metrics on HTTP Port. Prometheus than pulls the metrics from agent servers based on the configuration in 'prometheus.yml file' .

## Installing Node Exporter on an Agent Node

* On an agent node, Download the following script and run it as a root user:

`wget -O - https://raw.githubusercontent.com/in4it/prometheus-course/master/scripts/2-node-exporter.sh | bash`

* Make sure Node Exporter Service is running and accessible to the Prometheus Server. You can view the http page by visiting the following URL:

`http://yourip:9100/metrics`
