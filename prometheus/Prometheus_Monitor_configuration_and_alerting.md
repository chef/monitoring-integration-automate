
# Monitor configuration and alerting

## Enabling and Configure Metrics

Metrics are enabled to alert the team members and present the performance on the dashboards

### System Level Metrics
In Prometheus, the following exporters are enabled to collect required system level metrics for the Chef Automate HA
* Node Exporter

The Node exporter provides system level metrics e.g. CPU, disk, memory etc. Refer to the [node exporter documentation](https://github.com/prometheus/node_exporter) to learn full capabilities.

Refer to the [node exporter service configuration file](./exporter_service_files/node_exporter.service) for the chef managed Automate HA deployment. 

### Configure Application Level Monitoring

This section explains the process to monitor Automate HA application services.

* Install the blackbox exporter on the following servers.
  - All Chef Automate Frontend Servers
  - All Chef Infra Frontend Servers
  - Any other Server to Monitoring Load Balancers

* Refer to [blackbox exporter service](./exporter_service_files/blackbox_exporter.service) and [black box configuration](./exporter_configs/blackbox_exporter.yml) files.

* Configure prometheus.yml file on prometheus server to scape metrics for various chef services. Refer to [prometheus.yml](./prometheus.yml) for the following job configurations
  - chef-server-url - Monitors elastic load balancer for chef infra frontend servers
  - chef-automate-url -  Monitors elastic load balancer for chef automate frontend servers
  - chef-server-services.* - Monitors all services running on each chef infra frontend servers
  - automate-services.* - Monitors all services running on each chef automate frontend servers

### Configure Postgres Metrics
  The postgres exporter is installed on each node running postgres. Refer to [postgres exporter documentation](https://github.com/prometheus-community/postgres_exporter) to learn more about capabilities. Refer to the [postgres exporter services](./exporter_service_files/postgres_exporter.service) and [postgres exporter config](./exporter_configs/postgres_exporter.env) files

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
* The following steps provides the guidance to prepare various receivers for the alert manager to send alerts and alertmanager configurations. 


### Configure Alert Manager

* Perform the following steps to configure alert manager

```
mkdir /etc/alertmanager/
vi /etc/alertmanager/alertmanager.yml
```

* Based on Alert integration to Slack, MS teams, pager duty add the following sections under receiver section.

```
route:
  # A default receiver
  receiver: slack
```

* Refer to the [prometheus alert manager configuration documentation](https://prometheus.io/docs/alerting/latest/configuration/) for detailed options. 

Refer to the following integration sections provides guidance to integrate with alertmanager 

1. [Slack Integration](./prometheus_slack_Integration_and_Notification.md)
1. [PagerDuty Integration ](./prometheus_PagerDuty_Integration_and_Notification.md)
1. [MS Teams Integration](./prometheus_msteams_Integration_and_Notification.md)

* Create alert manager service

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

* Run the following commands to start and enable the service.
```
systemctl daemon-reload
systemctl start alertmanager
systemctl status alertmanager
systemctl enable alertmanager
```

## Configuring AlertManager with Prometheus

* Add the following configuration in Prometheus

```
vi /etc/prometheus/prometheus.yml
```

```
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - localhost:9093
rule_files:
  - "system_rules.yml"
```

* Creates all the system specific rules in the following file

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

Execute the following command to validate configurations.

```
promtool check config /etc/prometheus/prometheus.yml
```

Output
```
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


