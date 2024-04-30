# Prometheus Download, Installation, and Configuration

We recommend referring to Prometheus [documentation](https://prometheus.io/docs/introduction/first_steps/) on Prometheus server installation and configuration.

## Introduction to Prometheus

Prometheus is an open-source time series monitoring tool for managing various system resources and applications. It provides a multidimensional data model, the ability to query the collected data, and detailed reporting and data visualization through Grafana.

By default, Prometheus is enabled to collect metrics on the server where it is installed. However, with the help of node exporters, metrics can be collected from other resources like web servers, containers, databases, custom applications, and other third-party systems. This tutorial will show you how to install and configure Prometheus with node exporters on an EC2 instance. See Exporters and Integrations in the Prometheus documentation for a complete list of available exporters.

## Prometheus and Chef Automate

To monitor the Chef Automate HA infrastructure, a Prometheus server must be installed, and exporters must be installed and configured on the infrastructure nodes to get the possible configured metrics and other information that can be sent to the monitoring tool on the server side to track the health of the overall infrastructure.

## Contents

* Prometheus Server Setup

    1. [Complete the prerequisite](#step-1-complete-the-prerequisites)
    1. [Add users and local system directories to your EC2 instance](#step-2-add-users-and-local-system-directories-to-your-ec2-instance)
    1. [Download and Install Prometheus binary packages](#step-3-download-and-install-prometheus-binary-packages)
    1. [Configure Prometheus](#4-configure-prometheus)
    1. [Start Prometheus](#step-5-start-prometheus)
    1. [Start Node Exporter](#step-6-start-node-exporter)
    1. [Configure Prometheus with the Node Exporter data collector](#step-7-configure-prometheus-with-the-node-exporter-data-collector)

* Prometheus Agent (node-exporter) Setup

    1. [Complete the prerequisites](#step-1-complete-the-prerequisites-1)
    1. [Add users to your EC2 instance](#step-2-add-user-to-your-ec2-instance)
    1. [Download and Install Prometheus binary packages](#step-3-download-and-install-prometheus-agent-binary-packages)
    1. [Start Node Exporter](#step-4-start-node-exporter)
    1. [Reconfigure Prometheus with the Node Exporter data collector](#step-5-reconfigure-prometheus-with-the-node-exporter-data-collector)

## Step 1: Complete the prerequisites

Before you can install Prometheus, you must do the following:

1. Create an instance in EC2 or VM. We recommend using the Ubuntu 20.04 LTS blueprint for your instance.

1. Open ports 9090 and 9100 on the firewall of your new instance. Prometheus requires ports 9090 and 9100 to be open.

1. The Prometheus server will communicate to the Chef Automate HA server over the following ports. The firewall must allow the Prometheus server to escape the chef server on the following ports. For more details, refer to the [agent installation document](./Prometheus-agent-installation-configuration.md).

| Exporters        | Firewall Ports |
|------------------|----------------|
| Node             | 9100           |
| PostgreSQL       | 9101           |
| Nginx            | 9113           |
| Blackbox         | 9115           |
| OpenSearch       | 9200           |
| HTTPS            | 443            |

## Step 2: Add users and local system directories to your EC2 instance

Complete the following procedure to connect to your EC2 instance using SSH and add users and system directories. This procedure creates the following Linux user accounts:

* **Prometheus:** This account is used to install and configure the server environment.

* **Exporter:** This account is used to configure the node_exporter extension.

These user accounts are created for the sole purpose of management and, therefore, do not require additional user services or permissions beyond the scope of this setup. In this procedure, you also create directories for storing and managing the files, service settings, and data that Prometheus uses to monitor resources.

* Connect using SSH to the EC2 instance and enter the following commands individually to create two Linux user accounts, Prometheus and Exporter.

```sh
sudo useradd --no-create-home --shell /bin/false prometheus
sudo useradd --no-create-home --shell /bin/false exporter
```

* Enter the following commands one by one to create local system directories.

```sh
sudo mkdir /etc/prometheus /var/lib/prometheus
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```

## Step 3: Download and Install Prometheus binary packages

Complete the following procedure to download the Prometheus binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus downloads page](https://prometheus.io/download/).

* At the top of the page, in the Operating System dropdown, select Linux. For Architecture, select AMD64.

* Select download filters for Prometheus. Choose or right-click the Prometheus download link, and copy the link address to a text file on your computer. Do the same for the node_exporter download link that appears. You will use both copied addresses later in this procedure.

* Copy the download link for Prometheus

* Connect to your EC2 instance using SSH.

Enter the following command to change directories to your home directory.

```sh
cd ~
```

* Enter the following command to download the Prometheus binary packages to your instance.

```sh
curl -LO prometheus-download-address
```

* Replace prometheus-download-address with the address that you copied earlier in this procedure. The command should look like the following example when you add the address.

```sh
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.37.0/prometheus-2.37.0.linux-amd64.tar.gz
```

* Enter the following command to download the node_exporter binary packages to your instance.

```sh
curl -LO node_exporter-download-address
```

* Replace node_exporter-download-address with the address you copied in this procedure's previous step. The command should look like the following example when you add the address.

```sh
curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```

* Run the following commands one by one to extract the contents of the downloaded Prometheus and Node Exporter files.

```sh
tar -xvf prometheus-2.37.0.linux-amd64.tar.gz
tar -xvf node_exporter-1.3.1.linux-amd64.tar.gz
```

* Several subdirectories are created after the contents of the downloaded files are extracted.

* Enter the following commands one by one to copy the files extracted from the Prometheus and protocol to the /usr/local/bin programs directory.

```sh
sudo cp -p ./prometheus-2.37.0.linux-amd64/prometheus/usr/local/bin
sudo cp -p ./prometheus-2.37.0.linux-amd64/promtool/usr/local/bin
```

* Enter the following command to change the ownership of the Prometheus and protocol files to the Prometheus user you created earlier in this tutorial.

```sh
sudo chown prometheus:prometheus /usr/local/bin/prom*
```

* Enter the following commands one by one to copy the consoles and console_libraries subdirectories to `/etc/prometheus`. The `-r` option performs a recursive copy of all directories within the hierarchy.

```sh
sudo cp -r ./prometheus-2.37.0.linux-amd64/consoles /etc/prometheus
sudo cp -r ./prometheus-2.37.0.linux-amd64/console_libraries /etc/prometheus
```

* Enter the following commands one by one to change the ownership of the copied files to the Prometheus user you created earlier in this tutorial. The `-R` option performs a recursive ownership change for all of the files and directories within the hierarchy.

```sh
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```

* Enter the following commands one by one to copy the configuration file prometheus.yml to the /etc/prometheus directory and change the ownership of the copied file to the Prometheus user you created earlier in this tutorial.

```sh
sudo cp -p ./prometheus-2.37.0.linux-amd64/prometheus.yml /etc/prometheus
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```

* Enter the following command to copy the node_exporter file from the `./node_exporter*` subdirectory to the `/usr/local/bin` programs directory.

```sh
sudo cp -p ./node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin
```

* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.

```sh
sudo chown exporter:exporter /usr/local/bin/node_exporter
```

## Step 4: Configure Prometheus

Complete the following procedure to configure Prometheus. In this procedure, you open and edit the prometheus.yml file, which contains various settings for the Prometheus tool. Based on the settings you configure in the file, Prometheus establishes a monitoring environment.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a backup copy of the **prometheus.yml** file before opening and editing it.

```sh
sudo cp /etc/prometheus/prometheus.yml /etc/prometheus/prometheus.yml.backup
```

* Following are a few critical parameters that you might want to configure in the **prometheus.yml** file:

  * **scrape_interval:** Located under the global header, this parameter defines the time interval (seconds) for how often Prometheus collects or scrapes metric data for a given target. The worldwide tag indicates that this setting is universal for all Prometheus monitor resources. This setting also applies to exporters unless an individual exporter provides a different value that overrides the global value. You can keep this parameter set to its current value of 15 seconds.

  * **job_name:** Located under the scrape_configs header, this parameter is a label that identifies exporters in the result set of a data query or visual display. You can specify the value of a job name to reflect best the resources being monitored in your environment. For example, you can label a job for managing a website as business-web-app, or you can label a database as `mysql-db-1`. In this initial setup, you only monitor the Prometheus server so that you can keep the current Prometheus value.
  * **targets:** Located under the static_configs header, the targets set uses an `ip_addr:port` key-value pair to identify the location where a given exporter is running. You will change the default setting in steps 4–7 of this procedure.

**Note:** You don't need to configure this initial setup's alerting and rule_files parameters.

* Scroll and find the target parameter located under the static_configs header.

* Change the default setting to `<ip_addr>:9090`. Replace `<ip_addr>` with the static IP address of the instance.

* Save the *.yaml* file.

## Step 5: Start Prometheus

Complete the following procedure to start the Prometheus service on your instance.

* Connect to your EC2 instance using SSH.

* Enter the following command to start the Prometheus service.

```sh
sudo -u prometheus /usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus --web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries
```

* The command line outputs details on the startup process and other services. It should also indicate that the service is listening on port 9090.

* If the service doesn't start, see Step 1: Complete the prerequisites section of this tutorial for information about creating instance firewall rules to allow traffic on this port. For other errors, review the prometheus.yml file to confirm no syntax errors.

* After the running service is validated, press *Ctrl+C* to stop it.

* Enter the following command to open the systemd configuration file in vi. This file is used to start Prometheus.

```sh
sudo vi /etc/systemd/system/prometheus.service
```

Insert the following lines into the file.

```ruby
[Unit]
Description=PromServer
Wants=network-online.target
After=network-online.target
[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries
[Install]
WantedBy=multi-user.target
```

* The Linux systemd service manager uses the preceding instructions to start Prometheus on the server. When invoked, Prometheus runs as the Prometheus user and references the prometheus.yml file to load the configuration settings and store the time series data in the `/var/lib/prometheus` directory. You can run man systemd from the command line to see more information about the service.

* Save your changes and quit vi.

* Enter the following command to load the information into the systemd service manager.

```sh
sudo systemctl daemon-reload
```

* Enter the following command to restart Prometheus.

```sh
sudo systemctl start prometheus
```

* Enter the following command to check the status of the Prometheus service.

```sh
sudo systemctl status prometheus
```

* Enter the following command to enable Prometheus to start when the instance is booted.

```sh
sudo systemctl enable prometheus
```

* Open a web browser on your local computer and go to the following web address to view the Prometheus management interface. Web address: `http:<ip_addr>:9090`.

* Replace `<ip_addr>` with the static IP address of your EC2 instance. You should see a dashboard similar to the following example.

![dashboard](./images/dashboard.png)

## Step 6: Start Node Exporter

Complete the following procedure to start the Node Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for node_exporter using vi.

```sh
sudo vi /etc/systemd/system/node_exporter.service
```

* Add the following lines of text to the file. This will configure node_exporter with monitoring collectors for CPU load, file system usage, and memory resources.

```ruby
[Unit]
Description=NodeExporter
Wants=network-online.target
After=network-online.target
[Service]
User=exporter
Group=exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter --collector.disable-defaults \
--collector.meminfo \
--collector.loadavg \
--collector.filesystem
[Install]
WantedBy=multi-user.target
```

**Note:** These instructions turn off default machine metrics for Node Exporter. See the Prometheus node_exporter man page in the Ubuntu documentation for a complete list of metrics available for Ubuntu.

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

* If the service is launched successfully, you will receive an output similar to the following example.

```ruby
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
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=filesystem_common.go:113 level=info collector=filesystem msg="Parsed fla>
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=node_exporter.go:108 level=info msg="Enabled collectors"
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=node_exporter.go:115 level=info collector=filesystem
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=node_exporter.go:115 level=info collector=loadavg
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=node_exporter.go:115 level=info collector=meminfo
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=node_exporter.go:199 level=info msg="Listening on" address=:9100
Jun 19 12:11:33 ip-10-100-10-131 node_exporter[2015]: ts=2023-06-19T12:11:33.354Z caller=tls_config.go:195 level=info msg="TLS is disabled." http2=false
lines 1-19/19 (END)
```

* Enter the following command to enable Node Exporter to start when the instance is booted.

```sh
sudo systemctl enable node_exporter
```

## Step 7: Configure Prometheus with the Node Exporter data collector

Complete the following procedure to configure Prometheus with the Node Exporter data collector. You do this by adding a new job_name parameter for node_exporter in the `prometheus.yml` file.

* Connect to your EC2 instance using SSH.

* Add the following lines of text into the file below the existing - targets: ["<ip_addr>:9090"] parameter.

```ruby
- job_name: "node_exporter"
  static_configs:
    - targets: ["<ip_addr>:9100"]
```

* The modified parameter in the **prometheus.yml** file should look like the following example.

```ruby
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"
    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.
    static_configs:
      - targets: ["10.100.10.131:9090"]
  - job_name: "node-exporter"
    static_configs:
      - targets: ["10.100.10.131:9100"]
```

### Static configs for Node Exporter

**Note:** Node Exporter listens to port 9100 for the Prometheus server to scrape the data. Confirm that you followed the steps for creating instance firewall rules outlined in Step 1: Complete the prerequisites section of this tutorial.

* As with configuring the Prometheus job_name, replace `<ip_addr>` with the static IP address attached to your EC2 instance.

* Save your changes and quit vi.

* Enter the following command to restart the Prometheus service so that the changes to the configuration file can take effect.

```sh
sudo systemctl restart prometheus
```

* Enter the following command to check the status of the Prometheus service.

```sh
sudo systemctl status prometheus
```

* If the service appropriately restarted, you will receive output similar to the following:

```ruby
root@ip-10-100-10-131:/etc/prometheus# sudo systemctl status prometheus
● prometheus.service - PromServer
     Loaded: loaded (/etc/systemd/system/prometheus.service; enabled; vendor preset: enabled)
     Active: active (running) since Mon 2023-06-19 12:23:18 UTC; 1min 24s ago
   Main PID: 2090 (Prometheus)
      Tasks: 8 (limit: 9267)
     Memory: 21.7M
     CGroup: /system.slice/prometheus.service
             └─2090 /usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus/ --web.console.templates=/etc/>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.424Z caller=head.go:613 level=info component=tsdb msg="WAL segment loaded" segment=0 ma>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.432Z caller=head.go:613 level=info component=tsdb msg="WAL segment loaded" segment=1 ma>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.433Z caller=head.go:613 level=info component=tsdb msg="WAL segment loaded" segment=2 ma>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.433Z caller=head.go:619 level=info component=tsdb msg="WAL replay completed" checkpoint>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.436Z caller=main.go:993 level=info fs_type=EXT4_SUPER_MAGIC
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.436Z caller=main.go:996 level=info msg="TSDB started"
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.436Z caller=main.go:1177 level=info msg="Loading configuration file" filename=/etc/prom>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.439Z caller=main.go:1214 level=info msg="Completed loading of configuration file" filen>
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.439Z caller=main.go:957 level=info msg="Server is ready to receive web requests."
Jun 19 12:23:18 ip-10-100-10-131 prometheus[2090]: ts=2023-06-19T12:23:18.439Z caller=manager.go:941 level=info component="rule manager" msg="Starting rule manag>
lines 1-19/19 (END)
```

* Open a web browser on your local computer and go to the following web address to view the Prometheus management interface. Web address: `http:<ip_addr>:9090`

* Replace `<ip_addr>` with the static IP address of your EC2 instance. You should see a dashboard.

The Prometheus dashboard:

* In the main menu, choose the Status dropdown and select Targets.

* Targets menu option on the Prometheus dashboard

On the next screen, you should see two targets. The first target is the node_exporter metrics collector job, and the second is the Prometheus job.

Targets on the Prometheus dashboard:

![targets](./images/targets.png)

* The environment is now properly set up for collecting metrics and monitoring the server.

**NOTE:** Additional configurations for the Prometheus server will be required as more exports are installed. Refer to [Prometheus agent installation Guide](./Prometheus-agent-installation-configuration.md)
