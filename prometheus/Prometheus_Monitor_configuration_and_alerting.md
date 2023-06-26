# Monitor configuration and alerting


## Installing AlertManager
The following steps will provide guidance to configure Promethues AlertManager.


```
cd ~
```
https://prometheus.io/download/#alertmanager

```
curl -LO  https://github.com/prometheus/alertmanager/releases/download/v0.25.0/alertmanager-0.25.0.linux-amd64.tar.gz

tar -xvf alertmanager-0.25.0.linux-amd64.tar.gz
mv alertmanager-0.25.0.linux-amd64/alertmanager /usr/local/bin

mkdir /etc/alertmanager/
vi /etc/alertmanager/alertmanager.yml
```

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
TODO write enablecommand

## Configuring Alert
```
vi /etc/prometheus/prometheus.yml
```
# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - localhost:9093

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

In the promethus UI:

 * Got to Monitor -> new monitor and create a Custom monitor

 * There are options to create monitors based on Hosts. process, metrices, etc. For this use case, we are going to create a monitor for Hosts.

    ![Data_Dog_Metrices](images/Data_Dog_Metrices.png)

 * Pick hosts you want to monitor filtering those by tags which need to be added to the hosts at the time of creation.

    ![Metrics_Host_Monitor](images/Metrics_Host_Monitor.png)

 * Select the option to automatically resolve the alerts after a specified period of time or it needs to be resolved by manual intervention of an user.

 * There is the option to notify the team responsible for the infrastructure monitoring via email or slack or teams or pgerduty. The detailed steps on integrating promethus with these alerting applications are explianed in dedictaed sections.

 * promethus provides the flexibility of renotification of the same alert which can be configured to renotify in the alerting app multiple times after a fixed interval of time.

    ![Metrics_Host_Monitor](images/Notify.png)

 * Different alerts need to be set a priority level depending on the impact they create on the performance or functioning of the applications. Priority for the custom alerts should be set in order to address those accordingly meeting the SLAs.

 * Role based access control needs to be applied to the custom monitors and alerts crtaed which dictates Who can edit/delete my alert and eho will it be notified to.

    ![Metrics_Host_Monitor](images/Priority.png)