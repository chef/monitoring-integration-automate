# Introduction to Prometheus Exporter

Prometheus is an open source time series monitoring tool for managing a variety of system resources and applications. It provides a multidimensional data model, the ability to query the collected data, and detailed reporting and data visualization through Grafana.

By default, Prometheus exporter is enabled to collect metrics on the server where it is installed. With the help of various exporters, metrics can be collected from other resources like web servers, containers, databases, custom applications, and other third-party systems. In this tutorial, we will show you how to install and configure required Prometheus exporters to setup monitoring for Chef Automate HA implementation. For a full list of available exporters, see Exporters and integrations in the [Prometheus documentation](https://prometheus.io/docs/instrumenting/exporters/).

## Content References

1. [Prometheus exporter's pre-requisites](#prometheus-exporter-prerequisites)

    * [Configure Firewall prerequisites](#step-1-configure-firewall-prerequisites)

    * [Add users to your EC2 instance](#step-2-add-user-to-your-ec2-instance)

1. [Prometheus Exporter - node-exporter Setup](#prometheus-node-exporter-setup)

    * [Verify Pre-requisites](#step-1-verify-prerequisites)

    * [Download and Install Node exporter packages](#step-2-download-and-install-node_exporter-binary-packages)

    * [Start Node Exporter](#step-3-start-node-exporter)

    * [Configure Prometheus with the Node Exporter data collector](#step-4-configure-prometheus-for-node_exporter-data-collector)

1. [Prometheus Postgres exporter setup](#prometheus-postgres-exporter-setup)

    * [Verify Pre-requisites](#step-1-verify-prerequisites-1)

    * [Download, Install and Configure Prometheus postgres exporter](#step-2-download-install-and-configure-prometheus-postgres-exporter)

    * [Start postgres Exporter](#step-3-start-postgres-exporter)

    * [Configure Prometheus with the postgres Exporter data collector](#step-4-reconfigure-prometheus-with-the-postgres-exporter-data-collector)

1. [Prometheus OpenSearch Plugin Setup](#configure-prometheus-with-opensearch-plugin)

    * [Install OpenSearch plugin](#step-1--install-opensearch-plugin)

    * [Reconfigure Prometheus Server](#step-2-configure-prometheus-server-for-opensearch-data-collection)

    * [Verify OpenSearch Metrics](#step-3-verify-opensearch-metrics)

1. [Prometheus Exporter - blackbox_exporter Setup](#configure-blackbox-exporter-for-website-monitoring)

    * [Verify Pre-requisites](#step-1-verify-prerequisites-2)

    * [Download and Install blackbox exporter packages](#step-2-download-install-and-configure-prometheus-blackbox-exporter)

    * [Start Blackbox Exporter](#step-3-start-blackbox-exporter)

    * [Configure Prometheus with the Blackbox Exporter data collector](#step-4-configure-prometheus-with-blackbox-data-collection)

1. [Prometheus Exporter - nginx-exporter Setup](#configure-nginx-exporter)

    * [Verify Pre-requisites](#step-1-verify-prerequisites-3)

    * [Download and Install Nginx exporter packages](#step-2-download-install-and-configure-prometheus-nginx-exporter)

    * [Start Nginx Exporter](#step-3-start-nginx-exporter)

    * [Configure Prometheus with the Nginx Exporter data collection](#step-4-configure-prometheus-with-nginx-exporter-data-collection)

## Prometheus Exporter Prerequisites

Before you can install Prometheus exporters on any Chef Automate nodes, you must do the following:

### Step 1: Configure Firewall Prerequisites

* Each Prometheus exporter run on a separate ports. Refer to the exporter's documentation for default port configurations. The following exporters and the respective ports are used in this setup. Please ensure that all required ports are open from prometheus server to exporters.

| Exporters          | Firewall Ports |
|------------------|----------------|
| Node             | 9100           |
| Postgres         | 9101           |
| Nginx            | 9113           |
| Blackbox         | 9115           |
| Opensaerch       | 9200           |
| https            | 443            |

### Step 2: Add users and local system directories to your EC2 instance

Complete the following procedure to connect to your EC2 instance using SSH and add users and system directories as needed. This procedure creates the following Linux user accounts:

* Exporter: This account is used to configure the node_exporter extension.

These user accounts are created for the sole purpose of management and therefore do not require additional user services or permissions beyond the scope of this setup. In this procedure, you also create directories for storing and managing the files, service settings, and data that Prometheus uses to monitor resources.

* Connect using SSH to the EC2 instance and enter the following commands one by one to create two Linux user accounts, prometheus and exporter.

```sh
sudo useradd --no-create-home --shell /bin/false exporter
```

## Prometheus Node Exporter Setup

### Scope

These steps will be repeated on all of the following servers.

* Automate node
* Chef Infra Server
* Chef managed OpenSearch
* Chef managed Postgres
* Bastion node

### Step 1: Verify Prerequisites

Ensure that exporter pre-requisite configuration is completed. Refer to [Pre-requisites section](#prometheus-exporter-prerequisites)

### Step 2: Download and Install node_exporter binary packages

Complete the following procedure to download the Prometheus node exporter binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus downloads page](https://prometheus.io/docs/instrumenting/exporters/).

* Copy download link for node exporter.

* Connect to your EC2 instance using SSH.

Enter the following command to change directory to your home directory.

```sh
cd ~
```

* Enter the following command to download the node_exporter binary packages to your instance.

```sh
curl -LO node_exporter-download-address
```

* Replace node_exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.

```sh
curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```

* Run the following command extract the contents of the downloaded Node Exporter files.

```sh
tar -xvf node_exporter-1.3.1.linux-amd64.tar.gz
```

* Several subdirectories/files are created after the contents of the downloaded files are extracted.

* Enter the following command to copy the node_exporter file from the ./node_exporter* subdirectory to the /usr/local/bin programs directory.

```sh
sudo cp -p ./node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin
```

* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.

```sh
sudo chown exporter:exporter /usr/local/bin/node_exporter
```

### Step 3: Start Node Exporter

Complete the following procedure to start the Node Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for node_exporter using vi.

```sh
sudo vi /etc/systemd/system/node_exporter.service
```

* Add the following lines of text into the file. This will configure node_exporter with monitoring collectors for CPU load, file system usage, and memory resources.

```sh
[Unit]
Description=NodeExporter
Wants=network-online.target
After=network-online.target

[Service]
User=exporter
Group=exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

* Save your changes and quit vi.

* Enter the following command to reload the systemd process.

```sh
sudo systemctl daemon-reload
```

* Enter the following command to start the node_exporter service.

```sh
sudo systemctl start node_exporter
```

* Enter the following command to check the status of the node_exporter service.

```sh
sudo systemctl status node_exporter
```

* If the service launched successfully, you receive an output similar to the following example.

```sh
root@ip-10-100-10-131:/etc/prometheus# sudo systemctl status node_exporter
● node_exporter.service - NodeExporter
     Loaded: loaded (/etc/systemd/system/node_exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-06-19 12:11:33 UTC; 2min 4s ago
   Main PID: 2015 (node_exporter)
      Tasks: 4 (limit: 9267)
     Memory: 2.0M
     CGroup: /system.slice/node_exporter.service
             └─2015 /usr/local/bin/node_exporter --collector.disable-defaults --collector.meminfo --collector.loadavg --collector.filesystem

Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.353Z caller=node_exporter.go:182 level=info msg="Starting node_exporter" version="(v>
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.353Z caller=node_exporter.go:183 level=info msg="Build context" build_context="(go=g>
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=filesystem_common.go:111 level=info collector=filesystem msg="Parsed fla>
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=node_exporter.go:115 level=info collector=meminfo
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false
lines 1-19/19 (END)
```

* Enter the following command to enable Node Exporter to start when the instance is booted.

```sh
sudo systemctl enable node_exporter
```

### Step 4: Configure Prometheus for node_exporter data collector

Complete the following procedure to configure Prometheus with the Node Exporter data collector. You do this by adding a new job_name parameter for node_exporter in the prometheus.yml file.

* Connect to your EC2 instance using SSH.

* Add the following lines of text into the file, below the existing - targets: ["<ip_addr>:9090"] parameter.

  - job_name: "node_exporter"
    static_configs:
      - targets: ["<ip_addr>:9100"]

* The modified parameter in the prometheus.yml file should look like the following example.

scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "node-exporter"
    static_configs:
      - targets: ["10.100.10.131:9100"]

Static configs for Node Exporter

* As with the configuration of the prometheus job_name, replace <ip_addr> with the static IP address that's attached to your EC2 instance.

* Save your changes and quit vi.

* Enter the following command to restart the Prometheus service so that the changes to the configuration file can take effect.

```sh
sudo systemctl restart prometheus
```

* Enter the following command to check the status of the Prometheus service.

```sh
sudo systemctl status prometheus
```

* If the service restarted properly, you receive output similar to the following.

```sh
root@ip-10-100-10-131:/etc/prometheus# sudo systemctl status prometheus
● prometheus.service - PromServer
     Loaded: loaded (/etc/systemd/system/prometheus.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-06-19 12:23:18 UTC; 1min 24s ago
   Main PID: 2090 (prometheus)
      Tasks: 8 (limit: 9267)
     Memory: 21.7M
     CGroup: /system.slice/prometheus.service
             └─2090 /usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/ --web.console.templates=/etc/>

Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.424Z caller=head.go:613 level=info component=tsdb msg="WAL segment loaded" segment=0 ma>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.436Z caller=main.go:996 level=info msg="TSDB started"
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.436Z caller=main.go:1177 level=info msg="Loading configuration file" filename=/etc/prom>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.439Z caller=main.go:1214 level=info msg="Completed loading of configuration file" filen>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.439Z caller=main.go:957 level=info msg="Server is ready to receive web requests."
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.439Z caller=manager.go:941 level=info component="rule manager" msg="Starting rule manag>
lines 1-19/19 (END)
```

* Open a web browser on your local computer and go to the following web address to view the Prometheus management interface.

```sh
http:<ip_addr>:9090
```

* Replace <ip_addr> with the static IP address of your EC2 instance. You should see a dashboard.

The Prometheus dashboard:

* In the main menu, choose the Status dropdown and select Targets.

* Targets menu option on the Prometheus dashboard
On the next screen, you should see two targets. The first target is for the node_exporter metrics collector job, and the second target is for the prometheus job.

Targets on the Prometheus dashboard

![targets](./images/targets.png)

* The environment is now properly set up for collecting metrics and monitoring the server.

## Prometheus Postgres Exporter Setup

### Scope

* These steps will be repeated on all of the following servers.

    * Chef managed Postgres nodes

* An additional server may be configured with postgres exporter to collect metrics from AWS hosted postgres.

#### Step 1: Verify Prerequisites

Ensure that exporter pre-requisite configuration is completed. Refer to [Pre-requisites section](#prometheus-exporter-prerequisites)

#### Step 2: Download, Install and configure Prometheus postgres exporter

Complete the following procedure to download the Prometheus postgres-exporter binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus community postgres-exporter release page](https://github.com/prometheus-community/postgres_exporter/releases) .

* From the list, select the Operating system linux and Architecture amd64.

* Copy download link for postgres exporter

* Connect to your EC2 instance using SSH.

Enter the following command to change directories to your home directory.

```sh
cd ~
```

* Enter the following command to download the exporter binary packages to your instance.

curl -LO exporter-download-address

* Replace exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.

```sh
curl -LO https://github.com/prometheus-community/postgres_exporter/releases/download/v0.12.1/postgres_exporter-0.12.1.linux-amd64.tar.gz
```

* Run the following command(s) one by one to extract the contents of the downloaded Exporter files.

```sh
tar -xvf postgres_exporter-0.12.1.linux-amd64.tar.gz
```

* Several subdirectories are created after the contents of the downloaded files are extracted.

* Enter the following command to copy the exporter file from the ./exporter* subdirectory to the /usr/local/bin programs directory.

```sh
cp -p ./postgres_exporter-0.12.1.linux-amd64/postgres_exporter /usr/local/bin/
```

* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.

```sh
sudo chown exporter:exporter /usr/local/bin/postgres_exporter
```

* Identify the postgres leader by running the following command on the bastion host.

```sh
chef-automate status
```

* Connect to the leader node via ssh and connect  to the postgres sql to create user with required permissions with the following commands.

**Note:** DO NOT repeat these commands on follower nodes of postgres cluster.

```sh
/hab/pkgs/core/postgresql-client/9.6.24/20220311205413/bin/psql --host=localhost --dbname=postgres --username=admin
```

Use the default admin password to connect and then execute the following command at the psql prompt.

```sh
create user prometheus with password 'prometheus';
grant SELECT ON pg_stat_database to prometheus;
grant pg_monitor to prometheus;
```

* Enter the following command to create environment file for the exporter.

```sh
mkdir /opt/postgres_exporter
sudo vi /opt/postgres_exporter/postgres_exporter.env
```

* Add the following content to the postgres-exporter environment file.

```sh
DATA_SOURCE_NAME="postgresql://prometheus:prometheus@localhost:5432/postgres"
```

#### Step 3: Start Postgres Exporter

Complete the following procedure to start the Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for exporter using vi.

```sh
vi /etc/systemd/system/postgres_exporter.service
```

* Add the following lines of text into the file to configure exporter as a service. Update the ip address of postgres node in the following parameters as per your environment.

web.listen-address=10.100.12.65:9101
[Unit]
Description=Prometheus exporter for Postgresql
Wants=network-online.target
After=network-online.target
[Service]
User=exporter
Group=exporter
WorkingDirectory=/opt/postgres_exporter
EnvironmentFile=/opt/postgres_exporter/postgres_exporter.env
ExecStart=/usr/local/bin/postgres_exporter --web.listen-address=10.100.12.65:9101 --web.telemetry-path=/metrics
Restart=always
[Install]
WantedBy=multi-user.target

* Save your changes and quit vi.

* Enter the following command to reload the systemd process.

```sh
sudo systemctl daemon-reload
```

* Enter the following command to start the exporter service.

```sh
sudo systemctl start postgres_exporter
```

* Enter the following command to check the status of the exporter service.

```sh
sudo systemctl status postgres_exporter
```

* If the service launched successfully, you receive an output similar to the following example.

```sh
root@ip-10-100-12-65:~# sudo systemctl status postgres_exporter
● postgres_exporter.service - Prometheus exporter for Postgresql
     Loaded: loaded (/etc/systemd/system/postgres_exporter.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2023-06-21 07:56:12 UTC; 35min ago
   Main PID: 36586 (postgres_export)
      Tasks: 5 (limit: 9267)
     Memory: 7.0M
     CGroup: /system.slice/postgres_exporter.service
             └─36586 /usr/local/bin/postgres_exporter --web.listen-address=10.100.12.65:9101 --web.telemetry-path=/metrics

Jun 21 07:56:12 ip-10-100-12-65 systemd[1]: Started Prometheus exporter for Postgresql.
Jun 21 07:56:12 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:56:12.637Z caller=main.go:86 level=error msg="Error loading c>
Jun 21 07:56:12 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:56:12.637Z caller=tls_config.go:235 level=info msg="TLS is di>
Jun 21 07:58:52 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:58:52.564Z caller=server.go:74 level=info msg="Established ne>
Jun 21 07:58:52 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:58:52.574Z caller=postgres_exporter.go:647 level=info msg="Se>
lines 1-16/16 (END)
```

* Enter the following command to enable the exporter to start when the instance is booted.

```sh
sudo systemctl enable postgres_exporter
```

## Step 4: Configure Prometheus with the postgres Exporter data collector

Complete this procedure to reconfigure Prometheus server with exporter data collector for each postgres node.

* Append the /etc/prometheus/prometheus.yml file with the following content

  - job_name: "postgres-exporter-node-01"
    static_configs:
      - targets: ["10.100.12.65:9101"]

* Enter the following command to restart the Prometheus service so that the changes to the configuration file can take effect.

```sh
sudo systemctl restart prometheus
```

* Enter the following command to check the status of the Prometheus service.

```sh
sudo systemctl status prometheus
```

## Configure Prometheus with OpenSearch Plugin

### Scope

These steps will be repeated on all of the following servers.
    * Chef managed OpenSearch

#### Step 1 : Install OpenSearch Plugin

Refer to the [Prometheus exporter OpenSearch Plugin](https://github.com/aiven/prometheus-exporter-plugin-for-opensearch/releases).

** Disclaimer **

* The plugin is not supported by prometheus community. However, it is supported by OpenSearch community. Please refer to the plugin document for any future changes.

* The following steps provides guidance to install and configure OpenSearch plugin for chef managed OpenSearch environment only.

* Verify the OpenSearch version installed on chef manages OpenSearch nodes.

```sh
opensearch -V
```

* Chef automate HA uses OpenSearch version 1.3.7 at the time of this documentation creation.

* Copy the link for the correct version from [Prometheus exporter OpenSearch Plugin repo](https://github.com/aiven/prometheus-exporter-plugin-for-opensearch/tags).

* Execute the following steps on each OpenSearch node.

* Replace the link in the following command and install the plugin

```sh
opensearch-plugin install https://github.com/aiven/prometheus-exporter-plugin-for-opensearch/releases/download/1.3.7.0/prometheus-exporter-1.3.7.0.zip
```

* Reboot the OpenSearch node

```sh
reboot
```

#### Step 2: Configure Prometheus Server for OpenSearch data collection

* After installing the OpenSearch plugin, configure the prometheus server to scrape OpenSearch nodes

```sh
vi /etc/prometheus/prometheus.yml
```

* Add the following OpenSearch configuration in /etc/prometheus/prometheus.yml file as per environment.

- job_name: OpenSearch
    scrape_interval: 10s
    metrics_path: "_prometheus/metrics"
    scheme: https
    tls_config:
      insecure_skip_verify: true
    basic_auth:
      username: 'admin'
      password: 'admin'
    static_configs:
      - targets:
        - 10.100.8.152:9200
        - 10.100.8.233:9200
        - 10.100.8.187:9200

* Restart the prometheus service.

```sh
systemctl restart prometheus.service
```

#### Step 3: Verify OpenSearch metrics

* Browse to the prometheus url, click on Graph, Start typing OpenSearch in expression and OpenSearch metrics should start appearing as shown below

![OpenSearch](./images/opensearch.png)

## Configure Blackbox exporter for web based monitoring

### Scope

These steps will be repeated on all of the following servers.

* Automate node
* Chef Infra Server

#### Step 1: Verify Prerequisites

Ensure that exporter pre-requisite configuration is completed. Refer to [Pre-requisites section](#prometheus-exporter-prerequisites)

#### Step 2: Download, Install and configure Prometheus blackbox exporter

Complete the following procedure to download the Prometheus blackbox-exporter binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus community blackbox-exporter release page](https://github.com/prometheus/blackbox_exporter/releases) .

* From the list, select the Operating system linux and Architecture amd64.

* Copy download link for Prometheus
* Connect to your EC2 instance using SSH.

Enter the following command to change directories to your home directory.

```sh
cd ~
```

* Enter the following command to download the exporter binary packages to your instance.

curl -LO exporter-download-address

* Replace exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.

```sh
curl -LO https://github.com/prometheus/blackbox_exporter/releases/download/v0.24.0/blackbox_exporter-0.24.0.linux-amd64.tar.gz
```

* Run the following command(s) one by one to extract the contents of the downloaded Exporter files.

```sh
tar -xvf blackbox_exporter-0.24.0.linux-amd64.tar.gz
```

* Several subdirectories/files are created after the contents of the downloaded files are extracted.

* Enter the following command to copy the exporter file from the ./exporter* subdirectory to the /usr/local/bin programs directory.

```sh
cp blackbox_exporter-0.24.0.linux-amd64/blackbox_exporter /usr/local/bin/
```

* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.

```sh
sudo chown exporter:exporter /usr/local/bin/blackbox_exporter
```

* Enter the following command to create environment file for the exporter.

```sh
mkdir /opt/blackbox_exporter
sudo vi /opt/blackbox_exporter/blackbox.yml
```

* Add the following content to the yaml file.

Update the following parameters for your environment.

```sh
modules:
  http_2xx:
    prober: http
    http:
      valid_http_versions: ["HTTP/1.1", "HTTP/2"]
      method: "GET"
      headers:
        Host: vhost.example.com
      tls_config:
        insecure_skip_verify: true
      valid_status_codes: [200]
```

#### Step 3: Start Blackbox Exporter

Complete the following procedure to start the Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for exporter using vi.

```sh
vi /etc/systemd/system/blackbox_exporter.service
```

* Add the following lines of text into the file to configure exporter as a service.

[Unit]
Description=BlackBox Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=exporter
Group=exporter
Type=simple
ExecStart=/usr/local/bin/blackbox_exporter --config.file=/opt/blackbox_exporter/blackbox.yml

[Install]
WantedBy=multi-user.target

* Save your changes and quit vi.

* Enter the following command to reload the systemd process.

```sh
sudo systemctl daemon-reload
```

* Enter the following command to start the exporter service.

```sh
sudo systemctl start blackbox_exporter
```

* Enter the following command to check the status of the exporter service.

```sh
sudo systemctl status blackbox_exporter
```

* Enter the following command to enable Blackbox Exporter to start when the instance is booted.

```sh
sudo systemctl enable blackbox_exporter
```

#### Step 4: Configure Prometheus with blackbox data collection

Complete this procedure to reconfigure Prometheus server with exporter data collector.

* Append the /etc/prometheus/prometheus.yml file with the following content.

### For Chef URLS

Refer to [prometheus.yml](./prometheus.yml) job_name: 'chef-automate-url' and job_name: 'chef-server-url' configurations

### For automate services configuration

Refer to [prometheus.yml](./prometheus.yml) job_name: 'automate-services' configurations

### For chef server services configuration

Refer to [prometheus.yml](./prometheus.yml) job_name: 'chef-server-services' configurations

* Enter the following command to restart the Prometheus service so that the changes to the configuration file can take effect.

```sh
sudo systemctl restart prometheus
```

* Enter the following command to check the status of the Prometheus service.

```sh
sudo systemctl status prometheus
```

## Configure Nginx exporter

### Scope

These steps will be repeated on Automate node server.

#### Step 1: Verify Prerequisites

Ensure that exporter pre-requisite configuration is completed. Refer to [Pre-requisites section](#prometheus-exporter-prerequisites)

#### Step 2: Download, Install and configure Prometheus nginx exporter

Complete the following procedure to download the Prometheus blackbox-exporter binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [nginx-exporter release page](https://github.com/nginxinc/nginx-prometheus-exporter/releases) .

* From the lit, select the Operating system linux and Architecture amd64.

* Copy download link for Prometheus

* Connect to your EC2 instance using SSH.

Enter the following command to change directories to your home directory.

```sh
cd ~
```

* Enter the following command to download the exporter binary packages to your instance.

curl -LO exporter-download-address

* Replace exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.

```sh
curl -LO https://github.com/nginxinc/nginx-prometheus-exporter/releases/download/v0.11.0/nginx-prometheus-exporter_0.11.0_linux_amd64.tar.gz
```

* Run the following command(s) one by one to extract the contents of the downloaded Exporter files.

```sh
tar -xvf nginx-prometheus-exporter_0.11.0_linux_amd64.tar.gz
```

* Several subdirectories are created after the contents of the downloaded files are extracted.

* Enter the following command to copy the exporter file from the ./exporter* subdirectory to the /usr/local/bin programs directory.

```sh
cp nginx-prometheus-exporter /usr/local/bin/
```

* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.

```sh
sudo chown exporter:exporter /usr/local/bin/nginx-prometheus-exporter
```

#### Step 3: Start nginx Exporter

Complete the following procedure to start the Exporter service.

* Connect to your EC2 instance using SSH.
* Enter the following command to create environment file for the exporter.

```sh
mkdir /opt/nginx-prometheus-exporter
sudo vi /opt/nginx-prometheus-exporter/nginx-prometheus-exporter.env
```

* Add the following content to the nginx-prometheus-exporter environment file.

```sh
SSL_VERIFY=false
```

* Enter the following command to create a systemd service file for exporter using vi.

```sh
vi /etc/systemd/system/nginx-prometheus-exporter.service
```

* Add the following lines of text into the file to configure exporter as a service.

[Unit]
Description=Nginx Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=exporter
Group=exporter
Type=simple
EnvironmentFile=/opt/nginx-prometheus-exporter/nginx-prometheus-exporter.env
ExecStart=/usr/local/bin/nginx-prometheus-exporter -nginx.scrape-uri=https://localhost/nginx_status/

[Install]
WantedBy=multi-user.target

* Save your changes and quit vi.

* Enter the following command to reload the systemd process.

```sh
sudo systemctl daemon-reload
```

* Enter the following command to start the exporter service.

```sh
sudo systemctl start nginx-prometheus-exporter
```

* Enter the following command to check the status of the exporter service.

```sh
sudo systemctl status nginx-prometheus-exporter
```

* Enter the following command to enable Nginx Exporter to start when the instance is booted.

```sh
sudo systemctl enable nginx-prometheus-exporter
```

#### Step 4: Configure Prometheus with nginx Exporter data collection

Complete this procedure to reconfigure Prometheus server with exporter data collector.

* Append the /etc/prometheus/prometheus.yml file with the following content.

  - job_name: "chef-automate-nginx-node-01"
    static_configs:
      - targets: ["10.100.4.233:9113"]

* Enter the following command to restart the Prometheus service so that the changes to the configuration file can take effect.

```sh
sudo systemctl restart prometheus
```

* Enter the following command to check the status of the Prometheus service.

```sh
sudo systemctl status prometheus
```
