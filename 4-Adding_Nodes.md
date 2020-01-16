# Adding Nodes to Prometheus Server

## Adding Nodes to Prometheus Server using EC2 Auto Discovery Feature

* EC2 AutoDiscovery feature adds AWS instances to Prometheus based on a specific regex. To add EC2 instances, first rename your instances in AWS starting with a specific regex. Example: 'prod-*' or 'stage-*' .
* Add the following lines under scrape_configs to `prometheus.yml` :

```
- job_name: 'node'
      ec2_sd_configs:
        - region: eu-west-1
          access_key: KJHKJKJKJKHJKHKJJKK
          secret_key: 7KJJKHJKHJKHJKHJKHKJKJHJK
          port: 9100
      relabel_configs:
          # Only monitor instances with a Name starting with "SD Demo"
        - source_labels: [__meta_ec2_tag_Name]
          regex: prod-.*
          action: keep
        - source_labels: [__meta_ec2_tag_Name]
          target_label: instance
```
*Replace the above keys with your AWS IAM Keys*
**EC2 Read only Access is must for Prometheus to Read Your Instance Information**

## Adding a Standalone Server to Prometheus

If you want to add a server manually, you can define it as well in `prometheus.yml` file as follows:

```
- job_name: 'mystaticserver'
      static_configs:
      - targets: ['serverurl:9100']
```
*Multiple targets can be defined above as well*

**Note: Prometheus only pulls from the servers based on configuration defined in prometheus.yml file. Please make sure it is configured correctly as well as Node exporter is installed on all instances you want to monitor**



