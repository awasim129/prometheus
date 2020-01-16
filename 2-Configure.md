# Configuration

## 1. Prometheus

After Installing, make sure that prometheus and Alert Manager are running, then configure alert manager in `/etc/prometheus/prometheus.yml` as follows:

```# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
       - localhost:9093
```
In `/etc/prometheus/prometheus.yml` , add the following lines for a custom alert.rules file:
```
# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  - "/etc/prometheus/alert.rules"
```
*You may also refer to the prometheus.yml file in the conf directory of this repo regarding the above configuration.*

## 2 . AlertManager

Next, configure Notification Channels in `/etc/alertmanager/alertmanager.yml` .

* To Set Email notifications, use the following configuration:
```
global:
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

* You can also add Slack Notifications. To do so, add the following in `/etc/alertmanager/alertmanager.yml`

```
receivers:
- name: default
  slack_configs:
  - api_url: 'https://hooks.slack.com/services/XXXXYYYYZZ/AJKHKJHKJHJKHJKHKJHKJJKKJJK'
    channel: '#yourchannelname'
    send_resolved: true
    title: |-
      [{{ .Status | toUpper }}{{ if eq .Status "firing" }}:{{ .Alerts.Firing | len }}{{ end }}] {{ .CommonLabels.alertname }} for {{ .CommonLabels.job }}
      {{- if gt (len .CommonLabels) (len .GroupLabels) -}}
        {{" "}}(
        {{- with .CommonLabels.Remove .GroupLabels.Names }}
          {{- range $index, $label := .SortedPairs -}}
            {{ if $index }}, {{ end }}
            {{- $label.Name }}="{{ $label.Value -}}"
          {{- end }}
        {{- end -}}
        )
      {{- end }}
    text: >-
      {{ with index .Alerts 0 -}}
        :chart_with_upwards_trend: *<{{ .GeneratorURL }}|Graph>*
        {{- if .Annotations.runbook }}   :notebook: *<{{ .Annotations.runbook }}|Runbook>*{{ end }}
      {{ end }}

      *Alert details*:

      {{ range .Alerts -}}
        *Alert:* {{ .Annotations.title }}{{ if .Labels.severity }} - `{{ .Labels.severity }}`{{ end }}
      *Description:* {{ .Annotations.description }}
      *Details:*
        {{ range .Labels.SortedPairs }} • *{{ .Name }}:* `{{ .Value }}`
        {{ end }}
      {{ end }}
```
*You may also refer to the alertmanager.yml file in the conf directory of this repo regarding the above configuration.*

## 3. Grafana

For now, open Grafana on http://yourhost:3000 and change the password of admin user, we will configure it later.

