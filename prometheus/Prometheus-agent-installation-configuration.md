# Introduction to Prometheus
Prometheus is an open source time series monitoring tool for managing a variety of system resources and applications. It provides a multidimensional data model, the ability to query the collected data, and detailed reporting and data visualization through Grafana.

By default, Prometheus is enabled to collect metrics on the server where it is installed. With the help of node exporters, metrics can be collected from other resources like web servers, containers, databases, custom applications, and other third-party systems. In this tutorial, we will show you how to install and configure Prometheus with node exporters on a EC2 instance. For a full list of available exporters, see Exporters and integrations in the Prometheus documentation.

# Contents

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

    1. [ReConfigure Prometheus with the Node Exporter data collector](#step-5-reconfigure-prometheus-with-the-node-exporter-data-collector)

* Prometheus Postgres exporter setup
    1. [Complete the prerequisites](#step-1-complete-the-prerequisites-2)
    
    1. [Add users to your EC2 instance](#step-2-add-user-to-your-ec2-instance-1)

    1. [Download, Install and Configure Prometheus postgres exporter](#step-3-download-install-and-configure-prometheus-progres-exporter)

    1. [Start Progres Exporter](#step-4-start-progres-exporter)

    1. [ReConfigure Prometheus with the Progres Exporter data collector](#step-5-reconfigure-prometheus-with-the-progres-exporter-data-collector)


* Prometheus OpenSearch Plugin Setup
    1. [Install OpenSearch plugin](#step-1--install-opensearch-plugin)
    
    1. [Reconfigure Prometheus Server](#step-2-reconfigure-prometheus-server)

    1. [Verify OpenSearch Metrics](#step-3-verfiy-opensearch-metrics)

    1. [Start Progres Exporter](#step-4-start-progres-exporter)


## Step 1: Complete the prerequisites
  Before you can install Prometheus on an Amazon EC2 instance, you must do the following:

* Create an instance in EC2. We recommend using the Ubuntu 20.04 LTS blueprint for your instance. 

* Open ports 9090 and 9100 on the firewall of your new instance. Prometheus requires ports 9090 and 9100 to be open. 

## Step 2: Add users and local system directories to your EC2 instance
Complete the following procedure to connect to your EC2 instance using SSH and add users and system directories. This procedure creates the following Linux user accounts:

* prometheus – This account is used for installing and configuring the server environment.

* exporter – This account is used to configure the node_exporter extension.

These user accounts are created for the sole purpose of management and therefore do not require additional user services or permissions beyond the scope of this setup. In this procedure, you also create directories for storing and managing the files, service settings, and data that Prometheus uses to monitor resources.

* Connect using SSH to the EC2 instance and 
enter the following commands one by one to create two Linux user accounts, prometheus and exporter.
```
sudo useradd --no-create-home --shell /bin/false prometheus
sudo useradd --no-create-home --shell /bin/false exporter
```
* Enter the following commands one by one to create local system directories.
```
sudo mkdir /etc/prometheus /var/lib/prometheus
sudo chown prometheus:prometheus /etc/prometheus
sudo chown prometheus:prometheus /var/lib/prometheus
```

## Step 3: Download and Install Prometheus binary packages
Complete the following procedure to download the Prometheus binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus downloads page](https://prometheus.io/download/) .

* At the top of the page, for the Operating system dropdown, select linux. For Architecture, select amd64.

* Select download filters for Prometheus
Choose or right-click the Prometheus download link that appears, and copy the link address to a text file on your computer. Do the same for the node_exporter download link that appears. You will use both copied addresses later in this procedure.

* Copy download link for Prometheus
* * Connect to your EC2 instance using SSH.

Enter the following command to change directories to your home directory.
```
cd ~
```
* Enter the following command to download the Prometheus binary packages to your instance.

curl -LO prometheus-download-address

* Replace prometheus-download-address with the address that you copied earlier in this procedure. The command should look like the following example when you add the address.
```
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.37.0/prometheus-2.37.0.linux-amd64.tar.gz
```
* Enter the following command to download the node_exporter binary packages to your instance.

curl -LO node_exporter-download-address

* Replace node_exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.
```
curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```
* Run the following commands one by one to extract the contents of the downloaded Prometheus and Node Exporter files.
```
tar -xvf prometheus-2.37.0.linux-amd64.tar.gz
tar -xvf node_exporter-1.3.1.linux-amd64.tar.gz
```
* Several subdirectories are created after the contents of the downloaded files are extracted.

* Enter the following commands one by one to copy the prometheus and promtool extracted files to the /usr/local/bin programs directory.
```
sudo cp -p ./prometheus-2.37.0.linux-amd64/prometheus /usr/local/bin
sudo cp -p ./prometheus-2.37.0.linux-amd64/promtool /usr/local/bin
```
* Enter the following command to change the ownership of the prometheus and promtool files to the prometheus user that you created earlier in this tutorial.

sudo chown prometheus:prometheus /usr/local/bin/prom*

* Enter the following commands one by one to copy the consoles and console_libraries subdirectories to /etc/prometheus. The -r option performs a recursive copy of all directories within the hierarchy.
```
sudo cp -r ./prometheus-2.37.0.linux-amd64/consoles /etc/prometheus
sudo cp -r ./prometheus-2.37.0.linux-amd64/console_libraries /etc/prometheus
```
* Enter the following commands one by one to change the ownership of the copied files to the prometheus user that you created earlier in this tutorial. The -R option performs a recursive ownership change for all of the files and directories within the hierarchy.
```
sudo chown -R prometheus:prometheus /etc/prometheus/consoles
sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```
* Enter the following commands one by one to copy the configuration file prometheus.yml to the /etc/prometheus directory and change the ownership of the copied file to the prometheus user that you created earlier in this tutorial.
```
sudo cp -p ./prometheus-2.37.0.linux-amd64/prometheus.yml /etc/prometheus
sudo chown prometheus:prometheus /etc/prometheus/prometheus.yml
```
* Enter the following command to copy the node_exporter file from the ./node_exporter* subdirectory to the /usr/local/bin programs directory.
```
sudo cp -p ./node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin
```
* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.
```
sudo chown exporter:exporter /usr/local/bin/node_exporter
```
##  4: Configure Prometheus
Complete the following procedure to configure Prometheus. In this procedure, you open and edit the prometheus.yml file, which contains various settings for the Prometheus tool. Prometheus establishes a monitoring environment based on the settings that you configure in the file.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a backup copy of the prometheus.yml file before you open and edit it.
```
sudo cp /etc/prometheus/prometheus.yml /etc/prometheus/prometheus.yml.backup
```
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

* The preceding instructions are used by the Linux systemd service manager to start Prometheus on the server. When invoked, Prometheus runs as the prometheus user and references the prometheus.yml file for loading the configuration settings and storing the time series data in the /var/lib/prometheus directory. You can run man systemd from the command line to see more information about the service.

* Save your changes and quit Vim.

* Enter the following command to load the information into the systemd service manager.
```
sudo systemctl daemon-reload
```
* Enter the following command to restart Prometheus.
```
sudo systemctl start prometheus
```
* Enter the following command to check the status of the Prometheus service.
```
sudo systemctl status prometheus
```
* Enter the following command to enable Prometheus to start when the instance is booted.
```
sudo systemctl enable prometheus
```
* Open a web browser on your local computer and go to the following web address to view the Prometheus management interface.

http:<ip_addr>:9090

* Replace <ip_addr> with the static IP address of your EC2 instance. You should see a dashboard similar to the following example.  

![dashboard](./images/dashboard.png)

## Step 6: Start Node Exporter
Complete the following procedure to start the Node Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for node_exporter using Vim.
```
sudo vim /etc/systemd/system/node_exporter.service
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
ExecStart=/usr/local/bin/node_exporter --collector.disable-defaults \
--collector.meminfo \
--collector.loadavg \
--collector.filesystem

[Install]
WantedBy=multi-user.target
```
Note:
These instructions disable default machine metrics for Node Exporter. For a complete list of metrics available for Ubuntu, see the Prometheus node_exporter man page in the Ubuntu documentation.

* Save your changes and quit Vim.

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

```
sudo systemctl enable node_exporter
```
## Step 7: Configure Prometheus with the Node Exporter data collector
Complete the following procedure to configure Prometheus with the Node Exporter data collector. You do this by adding a new job_name parameter for node_exporter in the prometheus.yml file.

* Connect to your EC2 instance using SSH.

* Add the following lines of text into the file, below the existing - targets: ["<ip_addr>:9090"] parameter.
```
- job_name: "node_exporter"
  static_configs:
    - targets: ["<ip_addr>:9100"]
```
* The modified parameter in the prometheus.yml file should look like the following example.
``` 
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

Static configs for Node Exporter

Note the following:
Node Exporter listens to port 9100 for the prometheus server to scrape the data. Confirm that you followed the steps for creating instance firewall rules as outlined in the Step 1: Complete the prerequisites section of this tutorial.

* As with the configuration of the prometheusjob_name, replace <ip_addr> with the static IP address that's attached to your EC2 instance.

* Save your changes and quit Vim.

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

## Configure Prometheus Agent (node-exporter)
### Scope
    These steps will be repeated on all of the following servers.
      * Automate node
      * Chef Infra Server
      * Chef managed OpenSearch
      * Chef managed Postgres
      * Bastion node

## Step 1: Complete the prerequisites
  Before you can install Prometheus on an Amazon EC2 instance, you must do the following:

* Create an instance in EC2. We recommend using the Ubuntu 20.04 LTS blueprint for your instance. 

* Open ports 9100 on the firewall of your new instance. Prometheus Agent requires port 9100 to be open. 

## Step 2: Add user to your EC2 instance
Complete the following procedure to connect to your EC2 instance using SSH and add users. This procedure creates the following Linux user accounts:

* exporter – This account is used to configure the node_exporter extension.

These user accounts are created for user services or permissions beyond the scope of this setup.

* Connect using SSH to the EC2 instance and enter the following commands one by one to create two Linux user accounts, prometheus and exporter.
```
sudo useradd --no-create-home --shell /bin/false exporter
```

## Step 3: Download and Install Prometheus Agent binary packages
Complete the following procedure to download the Prometheus node-exporter binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus downloads page](https://prometheus.io/download/) .

* At the top of the page, for the Operating system dropdown, select linux. For Architecture, select amd64.

* Select download filters for Prometheus
Choose or right-click the Prometheus Agent (node-exporter) download link that appears, and copy the link address to a text file on your computer. 

* Copy download link for Prometheus
* * Connect to your EC2 instance using SSH.

Enter the following command to change directories to your home directory.
```
cd ~
```
* Enter the following command to download the node_exporter binary packages to your instance.

curl -LO node_exporter-download-address

* Replace node_exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.
```
curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
```
* Run the following command(s) one by one to extract the contents of the downloaded Node Exporter files.
```
tar -xvf node_exporter-1.3.1.linux-amd64.tar.gz
```
* Several subdirectories are created after the contents of the downloaded files are extracted.

* Enter the following command to copy the node_exporter file from the ./node_exporter* subdirectory to the /usr/local/bin programs directory.
```
sudo cp -p ./node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin
```
* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.
```
sudo chown exporter:exporter /usr/local/bin/node_exporter
```

## Step 4: Start Node Exporter
Complete the following procedure to start the Node Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for node_exporter using Vim.
```
sudo vim /etc/systemd/system/node_exporter.service
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
ExecStart=/usr/local/bin/node_exporter --collector.disable-defaults \
--collector.meminfo \
--collector.loadavg \
--collector.filesystem

[Install]
WantedBy=multi-user.target
```
Note:
These instructions disable default machine metrics for Node Exporter. For a complete list of metrics available for Ubuntu, see the Prometheus node_exporter man page in the Ubuntu documentation.

* Save your changes and quit Vim.

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

```
sudo systemctl enable node_exporter
```

## Step 5: ReConfigure Prometheus with the Node Exporter data collector
Complete this procedure to reconfigure Prometheus with each  Node Exporter data collector. Refer to the [Step 7: Configure Prometheus with the Node Exporter data collector](#step-7-configure-prometheus-with-the-node-exporter-data-collector).

## Configure Prometheus Postgres Exporter

## Step 1: Complete the prerequisites
  Before you can install Prometheus on an Amazon EC2 instance, you must do the following:

* Create an instance in EC2. We recommend using the Ubuntu 20.04 LTS blueprint for your instance. 

* Open ports 9101 on the firewall of your new instance. Prometheus progres exporter will requires port 9101 to be open.


## Step 2: Add user to your EC2 instance
Complete the following procedure to connect to your EC2 instance using SSH and add users. Skip this step of user already exists. This procedure creates the following Linux user accounts:

* exporter – This account is used to configure the exporter extension.

These user accounts are created for user services or permissions beyond the scope of this setup.

* Connect using SSH to the EC2 instance and enter the following commands to create two Linux user accounts, prometheus and exporter.

```
sudo useradd --no-create-home --shell /bin/false exporter
```

## Step 3: Download, Install and configure Prometheus progres exporter
Complete the following procedure to download the Prometheus postgres-exporter binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus community progres-exporter release page](https://github.com/prometheus-community/postgres_exporter/releases) .

* From the lit, select the Operating system linux and Architectur amd64.

* Copy download link for Prometheus
* * Connect to your EC2 instance using SSH.

Enter the following command to change directories to your home directory.
```
cd ~
```
* Enter the following command to download the exporter binary packages to your instance.

curl -LO exporter-download-address

* Replace exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.
```
curl -LO https://github.com/prometheus-community/postgres_exporter/releases/download/v0.12.1/postgres_exporter-0.12.1.linux-amd64.tar.gz
```
* Run the following command(s) one by one to extract the contents of the downloaded Exporter files.
```
tar -xvf postgres_exporter-0.12.1.linux-amd64.tar.gz
```
* Several subdirectories are created after the contents of the downloaded files are extracted.

* Enter the following command to copy the exporter file from the ./exporter* subdirectory to the /usr/local/bin programs directory.
```
cp -p ./postgres_exporter-0.12.1.linux-amd64/postgres_exporter /usr/local/bin/
```
* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.
```
sudo chown exporter:exporter /usr/local/bin/postgres_exporter
```

* Enter the following command to create environment file for the exporter.
```
mkdir /opt/postgres_exporter
sudo vi /opt/postgres_exporter/postgres_exporter.env
```

* Add the following content to the progres-exporter environment file.
Update the following parameters for your envionment. 

username: admin  
password: admin  
url: admin:admin@localhost  

```
DATA_SOURCE_NAME="postgresql://admin:admin@localhost:5432/?sslmode=disable"
```

## Step 4: Start Progres Exporter
Complete the following procedure to start the Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for exporter using Vim.
```
vi /etc/systemd/system/postgres_exporter.service
```
* Add the following lines of text into the file to configure exporter as a service. Update the ip address of postgres node in the following parameters as per your environment.  
web.listen-address=10.100.12.65:9101  

```
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
```

* Save your changes and quit Vim.

* Enter the following command to reload the systemd process.
```
sudo systemctl daemon-reload
```
* Enter the following command to start the exporter service.
```
sudo systemctl start postgres_exporter
```
* Enter the following command to check the status of the exporter service.
```
sudo systemctl status postgres_exporter
```
* If the service launched successfully, you receive an output similar to the following example.
```
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
Jun 21 07:56:12 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:56:12.637Z caller=proc.go:250 msg="Excluded databases" databa>
Jun 21 07:56:12 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:56:12.637Z caller=tls_config.go:232 level=info msg="Listening>
Jun 21 07:56:12 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:56:12.637Z caller=tls_config.go:235 level=info msg="TLS is di>
Jun 21 07:58:52 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:58:52.564Z caller=server.go:74 level=info msg="Established ne>
Jun 21 07:58:52 ip-10-100-12-65 postgres_exporter[36586]: ts=2023-06-21T07:58:52.574Z caller=postgres_exporter.go:647 level=info msg="Se>
lines 1-16/16 (END)
```

* Enter the following command to enable Node Exporter to start when the instance is booted.

```
sudo systemctl enable postgres_exporter
```

## Step 5: ReConfigure Prometheus with the Progres Exporter data collector
Complete this procedure to reconfigure Prometheus server with exporter data collector. 

* Append the /etc/prometheus/prometheus.yml file with the following content 
```
  - job_name: "progress-exporter-node-01"
    static_configs:
      - targets: ["10.100.12.65:9101"]
```

This change will require promethous services to be restarted. Refer to the [Step 7: Configure Prometheus with the Node Exporter data collector](#step-7-configure-prometheus-with-the-node-exporter-data-collector) for detailed steps.

## Configure Prometheus with OpenSearch Plugin

## Step 1 : Install OpenSearch Plugin
Refer to the [Prometheus exporter OpenSearch Plugin](https://github.com/aiven/prometheus-exporter-plugin-for-opensearch/releases). 

** Disclaimer ** 
  The plugin is not supported by prometheus community, However it is supported by Opensearch community. Please refer to the plugin document for any future changes.  
  
  The following steps provides guidance to install and configure opensearch plugin for chef managed opensearch environment only.

* Verify the opensearch version installed on chef manages opensearch nodes.  
```
opensearch -V
```

* Chef automate HA uses opensearch version 1.3.7 at the time of this documentation creation.

* Copy the link for the correct version from [Prometheus exporter OpenSeach Plugin repo](https://github.com/aiven/prometheus-exporter-plugin-for-opensearch/tags).

* Execute the following steps on each opensearch node.

* Replace the link in the following command and install the plugin

```
opensearch-plugin install https://github.com/aiven/prometheus-exporter-plugin-for-opensearch/releases/download/1.3.7.0/prometheus-exporter-1.3.7.0.zip
```

* Reboot the opensearch node

```
reboot
```


## Step 2: Reconfigure Prometheus Server

* After installing the opensearch plugin, configure the prometheus server to scrape opensearch nodes

```
vi /etc/prometheus/prometheus.yml
```

* Add the following opensearch configuration in /etc/prometheus/prometheus.yml file

```
- job_name: opensearch
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
```

* Restart the prometheus service.

```
systemctl restart prometheus.service
```

## Step 3: Verify opensearch metrics
* Browse to the prometheus url, click on Graph, Start typing opensearch in expression and opensearch metrics should start appearing as shown below

![opensearch](./images/opensearch.png)

## Configure Blackbox exporter for website monitoring

## Step 1: Complete the prerequisites
  Before you can install Prometheus on an Amazon EC2 instance, you must do the following:

* Create an instance in EC2. We recommend using the Ubuntu 20.04 LTS blueprint for your instance. 

* Open ports 9101 on the firewall of your new instance. Prometheus blackbox exporter will requires port 9115 to be open.


## Step 2: Add user to your EC2 instance
Complete the following procedure to connect to your EC2 instance using SSH and add users. Skip this step of user already exists. This procedure creates the following Linux user accounts:

* exporter – This account is used to configure the exporter extension.

These user accounts are created for user services or permissions beyond the scope of this setup.

* Connect using SSH to the EC2 instance and enter the following commands to create a Linux user account for exporter.

```
sudo useradd --no-create-home --shell /bin/false exporter
```

## Step 3: Download, Install and configure Prometheus blackbox exporter
Complete the following procedure to download the Prometheus blackbox-exporter binary packages to your EC2 instance.

* Open a web browser on your local computer and browse to the [Prometheus community blackbox-exporter release page](https://github.com/prometheus/blackbox_exporter/releases) .

* From the lit, select the Operating system linux and Architecture amd64.

* Copy download link for Prometheus
* * Connect to your EC2 instance using SSH.

Enter the following command to change directories to your home directory.
```
cd ~
```
* Enter the following command to download the exporter binary packages to your instance.

curl -LO exporter-download-address

* Replace exporter-download-address with the address that you copied in the previous step of this procedure. The command should look like the following example when you add the address.
```
curl -LO https://github.com/prometheus/blackbox_exporter/releases/download/v0.24.0/blackbox_exporter-0.24.0.linux-amd64.tar.gz
```
* Run the following command(s) one by one to extract the contents of the downloaded Exporter files.
```
tar -xvf blackbox_exporter-0.24.0.linux-amd64.tar.gz
```
* Several subdirectories are created after the contents of the downloaded files are extracted.

* Enter the following command to copy the exporter file from the ./exporter* subdirectory to the /usr/local/bin programs directory.
```
cp blackbox_exporter-0.24.0.linux-amd64/blackbox_exporter /usr/local/bin/
```
* Enter the following command to change the ownership of the file to the exporter user that you created earlier in this tutorial.
```
sudo chown exporter:exporter /usr/local/bin/blackbox_exporter
```

* Enter the following command to create environment file for the exporter.
```
mkdir /opt/blackbox_exporter
sudo vi /opt/blackbox_exporter/blackbox.yml
```

* Add the following content to the yaml file.
Update the following parameters for your environment. 

```
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

## Step 4: Start Blackbox Exporter
Complete the following procedure to start the Exporter service.

* Connect to your EC2 instance using SSH.

* Enter the following command to create a systemd service file for exporter using Vim.
```
vi /etc/systemd/system/blackbox_exporter.service
```
* Add the following lines of text into the file to configure exporter as a service. 

```
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
```

* Save your changes and quit Vim.

* Enter the following command to reload the systemd process.
```
sudo systemctl daemon-reload
```
* Enter the following command to start the exporter service.
```
sudo systemctl start blackbox_exporter
```
* Enter the following command to check the status of the exporter service.
```
sudo systemctl status blackbox_exporter
```
* Enter the following command to enable Blackbox Exporter to start when the instance is booted.

```
sudo systemctl enable blackbox_exporter
```

## Step 5: ReConfigure Prometheus with the Progres Exporter data collector


Complete this procedure to reconfigure Prometheus server with exporter data collector. 

* Append the /etc/prometheus/prometheus.yml file with the following content 
### For Chef URLS
Refer to [prometheus.yml](./prometheus.yml) job_name: 'chef-automate-url' and job_name: 'chef-server-url' configurations

### For automate services configuration
Refer to [prometheus.yml](./prometheus.yml) job_name: 'automate-services' configurations


### For chef server services configuration
Refer to [prometheus.yml](./prometheus.yml) job_name: 'chef-server-services' configurations


This change will require prometheus services to be restarted. Refer to the [Step 7: Configure Prometheus with the Node Exporter data collector](#step-7-configure-prometheus-with-the-node-exporter-data-collector) for detailed steps.

### For Chef Health Metrics