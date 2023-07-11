# Prometheus Download, Installation, and Configuration

We recommend to refer to Prometheus [documentation](https://prometheus.io/docs/introduction/first_steps/) on Prometheus server installation and configuration.

# Introduction to Prometheus
Prometheus is an open source time series monitoring tool for managing a variety of system resources and applications. It provides a multidimensional data model, the ability to query the collected data, and detailed reporting and data visualization through Grafana.

By default, Prometheus exporter is enabled to collect metrics on the server where it is installed. With the help of various exporters, metrics can be collected from other resources like web servers, containers, databases, custom applications, and other third-party systems. In this tutorial, we will show you how to install and configure required Prometheus exporters to setup monitoring for Chef Automate HA implementation. For a full list of available exporters, see Exporters and integrations in the [Prometheus documentation](https://prometheus.io/docs/instrumenting/exporters/).

# Prometheus and Chef Automate

In order to monitor the Chef Automate HA infrastructure, Prometheus server must be installed and exporters must be installed and configured on the infrastructure nodes to get the possible configured metrics and other information that can be sent to the monitoring tool on the server side in order to track the health of the overall infrastructure.

# Contents

* Prometheus Server Setup  
    
    1. [Complete the prerequisite](#step-1-complete-the-prerequisites)
    
    1. [Add users and local system directories to EC2 instance](#step-2-add-users-and-local-system-directories-to-your-ec2-instance)

    1. [Configure Firewall prerequisites](#step-1-configure-firewall-prerequisites)
    
    1. [Add users to your EC2 instance](#step-2-add-user-to-your-ec2-instance)

* [Prometheus Exporter - node-exporter Setup](#prometheus-node-exporter-setup)  
    
    1. [Verify Pre-requisites](#step-1-verify-prerequisites)

    1. [Download and Install Node exporter packages](#step-2-download-and-install-node_exporter-binary-packages)

    1. [Start Node Exporter](#step-3-start-node-exporter)

    1. [Configure Prometheus with the Node Exporter data collector](#step-4-configure-prometheus-for-node_exporter-data-collector)

* [Prometheus Postgres exporter setup](#prometheus-postgres-exporter-setup)
        
    1. [Verify Pre-requisites](#step-1-verify-prerequisites-1)

    1. [Download, Install and Configure Prometheus postgres exporter](#step-2-download-install-and-configure-prometheus-postgres-exporter)

    1. [Start postgres Exporter](#step-3-start-postgres-exporter)

    1. [Configure Prometheus with the postgres Exporter data collector](#step-4-reconfigure-prometheus-with-the-postgres-exporter-data-collector)


## Step 1: Complete the prerequisites
  Before you can install Prometheus, you must do the following:

* Create an instance in EC2 or VM. We recommend using the Ubuntu 20.04 LTS blueprint for your instance. 

* Each Prometheus exporter run on a separate ports. Refer to the exporter's documentation for default port configurations.  The following exporters and the respective ports are used in this setup. Please ensure that all required ports are open from prometheus server to exporters.

| Exporters          | Firewall Ports |
|------------------|----------------|
| Node             | 9100           |
| Postgres         | 9101           |
| Nginx            | 9113           |
| Blackbox         | 9115           |

## Step 2: Add users and local system directories to your EC2 instance
Complete the following procedure to connect to your EC2 instance using SSH and add users and system directories as needed. This procedure creates the following Linux user accounts:

* exporter – This account is used to configure the node_exporter extension.

These user accounts are created for the sole purpose of management and therefore do not require additional user services or permissions beyond the scope of this setup. In this procedure, you also create directories for storing and managing the files, service settings, and data that Prometheus uses to monitor resources.

* Connect using SSH to the EC2 instance and enter the following commands one by one to create two Linux user accounts, prometheus and exporter.
```
sudo useradd --no-create-home --shell /bin/false exporter
```

# Prometheus Node Exporter Setup

## Scope
    These steps will be repeated on all of the following servers.
      * Automate node
      * Chef Infra Server
      * Chef managed OpenSearch
      * Chef managed Postgres
      * Bastion node

## Step 1: Verify Prerequisites
Ensure that exporter pre-requisite configuration is completed. Refer to [Pre-requisites section](#prometheus-exporter-prerequisites)

## Step 2: Download and Install node_exporter binary packages
Complete the following procedure to download the Prometheus node exporter binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus downloads page](https://prometheus.io/docs/instrumenting/exporters/).

* Copy download link for node exporter

* Connect to your EC2 instance using SSH.

Enter the following command to change directory to your home directory.
```
cd ~
```

* Enter the following command to download the node_exporter binary packages to your instance.

curl -LO node_exporter-download-address

* Replace node_exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.
```
curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```
* Run the following command extract the contents of the downloaded Node Exporter files.
```
tar -xvf node_exporter-1.3.1.linux-amd64.tar.gz
```
* Several subdirectories/files are created after the contents of the downloaded files are extracted.

* Enter the following command to copy the node_exporter file from the ./node_exporter* subdirectory to the /usr/local/bin programs directory.
```
sudo cp -p ./node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin
```
* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.
```
sudo chown exporter:exporter /usr/local/bin/node_exporter
```
##  Step 4: Configure Prometheus
Complete the following procedure to configure Prometheus. In this procedure, you open and edit the prometheus.yml file, which contains various settings for the Prometheus tool. Prometheus establishes a monitoring environment based on the settings that you configure in the file.


* Following are a few important parameters that you might want to configure in the prometheus.yml file:

  * scrape_interval — Located under the global header, this parameter defines the time interval (in seconds) for how often Prometheus will collect or scrape metric data for a given target. As indicated by the global tag, this setting is universal for all resources that Prometheus monitors. This setting also applies for exporters, unless an individual exporter provides a different value that overrides the global value. You can keep this parameter set to its current value of 15 seconds.

  * job_name — Located under the scrape_configs header, this parameter is a label that identifies exporters in the result set of a data query or visual display. You can specify the value of a job name to best reflect the resources that are being monitored in your environment. For example, you can label a job for managing a website as business-web-app, or you can label a database as mysql-db-1. In this initial setup, you are only monitoring the Prometheus server, so you can keep the current prometheus value.

  * targets — Located under the static_configs header, the targets setting uses an ip_addr:port key-value pair to identify the location where a given exporter is running. You will change the default setting in steps 4–7 of this procedure.


Note:  
For this initial setup, you don't need to configure the alerting and rule_files parameters.

* Scroll and find the targets parameter located under the static_configs header.

* Change the default setting to <ip_addr>:9090. Replace <ip_addr> with the static IP address of the instance. 

* Save the yaml file.
  
## Step 5: Start Prometheus
Complete the following procedure to start the Prometheus service on your instance.

* Connect to your EC2 instance using SSH.

* Enter the following command to start the Prometheus service.

```
sudo -u prometheus /usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib/prometheus --web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/console_libraries
```

* The command line outputs details on the startup process and other services. It should also indicate that the service is listening on port 9090.


* If the service doesn't start, see the Step 1: Complete the prerequisites section of this tutorial for information about creating instance firewall rules to allow traffic on this port. For other errors, review the prometheus.yml file to confirm that there are no syntax errors.

* After the running service is validated, press Ctrl+C to stop it.

* Enter the following command to open the systemd configuration file in Vim. This file is used to start Prometheus.

sudo vim /etc/systemd/system/prometheus.service  

Insert the following lines into the file.

```
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

* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.

```
sudo chown exporter:exporter /usr/local/bin/node_exporter
```

## Step 3: Start Node Exporter
Complete the following procedure to start the Node Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for node_exporter using vi.
```
sudo vi /etc/systemd/system/node_exporter.service
```
* Add the following lines of text into the file. This will configure node_exporter with monitoring collectors for CPU load, file system usage, and memory resources.

```
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
```
sudo systemctl daemon-reload
```
* Enter the following command to start the node_exporter service.
```
sudo systemctl start node_exporter
```
* Enter the following command to check the status of the node_exporter service.
```
sudo systemctl status node_exporter
```
* If the service launched successfully, you receive an output similar to the following example.
```
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

```
sudo systemctl enable node_exporter
```
## Step 4: Configure Prometheus for node_exporter data collector
Complete the following procedure to configure Prometheus with the Node Exporter data collector. You do this by adding a new job_name parameter for node_exporter in the prometheus.yml file.

* Connect to your EC2 instance using SSH.

* Add the following lines of text into the file, below the existing - targets: ["<ip_addr>:9090"] parameter.
```
- job_name: "node_exporter"
  static_configs:
    - targets: ["<ip_addr>:9100"]
```

* The modified parameter in the prometheus.yml file should look like the following example.

Static configs for Node Exporter  

``` 
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "node-exporter"
    static_configs:
      - targets: ["10.100.10.131:9100"]
```

Note the following:
Node Exporter listens to port 9100 for the prometheus server to scrape the data. Confirm that you followed the steps for creating instance firewall rules as outlined in the Step 1: Complete the prerequisites section of this tutorial.

* As with the configuration of the prometheus job_name, replace <ip_addr> with the static IP address that's attached to your EC2 instance.

* Save your changes and quit vi.

* Enter the following command to restart the Prometheus service so that the changes to the configuration file can take effect.
```
sudo systemctl restart prometheus
```
* Enter the following command to check the status of the Prometheus service.
```
sudo systemctl status prometheus
```
* If the service restarted properly, you receive output similar to the following.
```
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

http:<ip_addr>:9090  

* Replace <ip_addr> with the static IP address of your EC2 instance. You should see a dashboard.

The Prometheus dashboard
* In the main menu, choose the Status dropdown and select Targets.

* Targets menu option on the Prometheus dashboard
On the next screen, you should see two targets. The first target is for the node_exporter metrics collector job, and the second target is for the prometheus job.

Targets on the Prometheus dashboard

![targets](./images/targets.png)

* The environment is now properly set up for collecting metrics and monitoring the server.

NOTE: Additional configurations for prometheus server will be required as more exporters are installed. Refer to [Prometheus agent installation Guide](./Prometheus-agent-installation-configuration.md)
