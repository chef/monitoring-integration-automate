
# Metrics and AlertManager Configuration

## Enabling and Configuring Metrics

Metrics can alert team members and present their performance on the dashboards. For details on collected metrics, refer to the [reference metrics list](./Prometheus_Reference_Metrics_List.md).

### Configure System Level Monitoring

In Prometheus, the following exporters are enabled to collect the required system-level metrics for the Chef Automate HA.

The Node exporter provides system-level metrics, such as CPU, disk, memory, etc. To learn its full capabilities, refer to the [node exporter documentation](https://github.com/prometheus/node_exporter).

For the chef-managed Automate HA deployment, refer to the [node exporter service configuration file](./exporter_service_files/node_exporter.service).

### Configure Application Level Monitoring

This section explains the process of monitoring Automate HA application services.

1. Install the Blackbox exporter on the following servers.

    * All Chef Automate Frontend Servers

    * All Chef Infra Frontend Servers

    * Any other Server to Monitoring Load Balancers

1. Refer to the [Blackbox Exporter Service](./exporter_service_files/blackbox_exporter.service) and [Blackbox configuration](./exporter_configs/blackbox_exporter.yml) files.

1. Configure the `prometheus.yml` file on the Prometheus server to capture metrics for various chef services. Refer to the [prometheus.yml](./prometheus.yml) for the following job configurations.

    * **chef-server-url:** Monitors elastic load balancer for Chef Infra Frontend servers.

    * **chef-automate-url:**  Monitors elastic load balancer for Chef Automate Frontend servers.

    * **chef-server-services.*:** Monitors all services running on each Chef Infra Frontend server.

    * **automate-services.*:** Monitors all services running on each chef to automate front-end servers.

### Configure PostgreSQL Metrics

The PostgreSQL exporter is installed on each node running PostgreSQL. Refer to [PostgreSQL exporter documentation](https://github.com/prometheus-community/postgres_exporter) to learn more about capabilities. Refer to the [PostgreSQL exporter services](./exporter_service_files/postgres_exporter.service) and [PostgreSQL exporter config](./exporter_configs/postgres_exporter.env) files

## Installing AlertManager

The following steps will guide you in configuring Prometheus AlertManager.

1. Change the current working path to the home directly

    ```sh
    cd ~
    ```

1. Refer to the [AlertManager Download Page](https://prometheus.io/download/#alertmanager) for the updated version of Alertmanager.

1. Execute the following command to download and install the Alertmanager:

    ```sh
    curl -LO  https://github.com/prometheus/alertmanager/releases/download/v0.25.0/alertmanager-0.25.0.linux-amd64.tar.gz
    tar -xvf alertmanager-0.25.0.linux-amd64.tar.gz
    mv alertmanager-0.25.0.linux-amd64/alertmanager /usr/local/bin
    ```

## Configure Alertmanager

### Prerequisites

* The following steps guide preparing various receivers for the alertmanager to send alerts and alertmanager configurations.

### Configuration

1. Perform the following steps to configure the alertmanager

    ```sh
    mkdir /etc/alertmanager/
    vi /etc/alertmanager/alertmanager.yml
    ```

1. Based on Alert integration to Slack, MS Teams, and Pager duty, add the following sections under the receiver section.

    ```sh
    Route:
        # A default receiver
        receiver: slack
    ```

1. Refer to the [Prometheus Alertmanager configuration Documentation](https://prometheus.io/docs/alerting/latest/configuration/) for detailed options.

Refer to the following integration sections to provide guidance to integrate with Alertmanager:

1. [Slack Integration](./prometheus_slack_Integration_and_Notification.md)

1. [PagerDuty Integration](./prometheus_PagerDuty_Integration_and_Notification.md)

1. [MS Teams Integration](./prometheus_msteams_Integration_and_Notification.md)

* Create an Alertmanager service by running the following command:

```sh
vi /etc/systemd/system/alertmanager.service
```

```sh
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

```sh
systemctl daemon-reload
systemctl start alertmanager
systemctl status alertmanager
systemctl enable alertmanager
```

## Configuring Alertmanager with Prometheus

1. Add the following configuration to Prometheus

    ```sh
    vi /etc/prometheus/prometheus.yml
    ```

    ```sh
    alerting:
        alertmanagers:
            - static_configs:
            - targets:
                - localhost:9093
            rule_files:
                - "system_rules.yml"
    ```

1. Creates all the system specific rules in the following file

    ```sh
    vi /etc/prometheus/system_rules.yml
    ```

    ```sh
    groups:
        - name: node-exporter
        rules:
            - alert: InstanceDown
                  expr: up == 0
            for: 1m
    ```

    Execute the following command to validate configurations.

    ```sh
    promtool check config /etc/prometheus/prometheus.yml
    ```

    Output:

    ```sh
    Checking /etc/prometheus/prometheus.yml
        SUCCESS: 1 rule file found
        SUCCESS: /etc/prometheus/prometheus.yml is a valid Prometheus config file syntax
    Checking /etc/prometheus/system_rules.yml
        SUCCESS: 1 rule found
    ```

    ```sh
    systemctl daemon-reload
    systemctl start prometheus
    systemctl status prometheus
    ```
