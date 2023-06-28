
# Monitor configuration and alerting


## Installing AlertManager
The following steps will provide guidance to configure Prometheus AlertManager.

* Change the current working path to the home directly
```
cd ~
```
* Refer to the [AlertManager Download Page](https://prometheus.io/download/#alertmanager) for more updated version of alert manager.

* Execute the following command to download and install the alert manager

```
curl -LO  https://github.com/prometheus/alertmanager/releases/download/v0.25.0/alertmanager-0.25.0.linux-amd64.tar.gz
tar -xvf alertmanager-0.25.0.linux-amd64.tar.gz
mv alertmanager-0.25.0.linux-amd64/alertmanager /usr/local/bin
```

## Configuring AlertManager

### Prerequisites
The following steps provides the guidance to prepare various receivers for the alert manager to send alerts and alertmanager configurations. 

1. Slack Notification
    Refer to this [guide](https://grafana.com/blog/2020/02/25/step-by-step-guide-to-setting-up-prometheus-alertmanager-with-slack-pagerduty-and-gmail/) for step-by-step guidance to configure Slack notification fo the prometheus alert manager.

1. PagerDuty Notification
    Refer to this [guide](https://grafana.com/blog/2020/02/25/step-by-step-guide-to-setting-up-prometheus-alertmanager-with-slack-pagerduty-and-gmail/) for step-by-step guidance to configure PagerDuty notification fo the prometheus alert manager.

1. MS teams notification
    Refer to this [guide](https://github.com/prometheus-msteams/prometheus-msteams) for step-by-step guidance to configure MSTeams notification fo the prometheus alert manager.


### Configure Alert Manager

Perform the following steps to configure alert manager

```
mkdir /etc/alertmanager/
vi /etc/alertmanager/alertmanager.yml
```

Based on Alert integration to Slack, MS teams, pager duty add the following sections under receiver section.

```
route:
  group_by: [Alertname]
  # Send all notifications to me.
  receiver: email-me

receivers:
- name: email-me
  email_configs:
  - to: receiver@gmail.com
    from: sender@gmail.com
    smarthost: smtp.gmail.com:587
    auth_username: "sender@gmail.com"
    auth_identity: "sender@gmail.com"
    auth_password: "**********************"
```

```
vi /etc/systemd/system/alertmanager.service
```

```
[Unit]
Description=AlertManager Server Service
Wants=network-online.target
After=network-online.target

[Service]
User=root
Group=root
Type=Simple
ExecStart=/usr/local/bin/alertmanager \
    --config.file /etc/alertmanager/alertmanager.yml

[Install]
WantedBy=multi-user.target
```
```
systemctl daemon-reload
systemctl start alertmanager
systemctl status alertmanager
```
TODO write enable command

## Configuring Alert
```
vi /etc/prometheus/prometheus.yml
```
# Alertmanager configuration

```
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - localhost:9093
```

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
```
rule_files:
  - "system_rules.yml"
```

```
vi /etc/prometheus/system_rules.yml
```

```
groups:
 - name: node-exporter
   rules:
   - alert: InstanceDown
     expr: up == 0
     for: 1m
```

```
promtool check config /etc/prometheus/prometheus.yml

Checking /etc/prometheus/prometheus.yml
  SUCCESS: 1 rule files found
 SUCCESS: /etc/prometheus/prometheus.yml is valid prometheus config file syntax

Checking /etc/prometheus/system_rules.yml
  SUCCESS: 1 rules found
```

```
systemctl daemon-reload
systemctl start prometheus
systemctl status prometheus
```


https://github.com/prometheus/node_exporter

https://grafana.com/blog/2020/02/25/step-by-step-guide-to-setting-up-prometheus-alertmanager-with-slack-pagerduty-and-gmail/

https://github.com/prometheus-msteams/prometheus-msteams
