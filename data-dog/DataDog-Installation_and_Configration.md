# Datadog Download, Installation, and Configuration

We recommend to refer to Datadog [documentation](https://docs.datadoghq.com/agent/) on Datadog agent installation and configuration as well.

## Datadog Download and Installation

### Description

In order to monitor the Chef Automate HA infrastructure, need to have a DataDog monitoring agent running on the infrastructure nodes to get the possible configured metrics and other information that can be sent to the monitoring tool on the server side in order to track the health of the overall infrastructure.

### Agent Definition

Agents for monitoring use cases refer to software that runs on your host machines. It collects events and metrics from hosts and sends them to the centralized monitoring tool’s infrastructure, where we can analyze the monitoring and performance data.

### Use case wise Requirements

1. Automate HA with On-Prem and AWS deployment

	Data dog agent to be configured on:

	* Automate node(s)
	* Chef Infra Server(s)
	* Chef-managed OpenSearch nodes
	* Chef-managed Postgres nodes
	* Bastion node

2. Automate HA with AWS deployment with Managed services

	* Data dog agent to be configured on:
    	* Automate node
    	* Chef Infra Server

	* AWS account integration with DataDog

	* Postgres YML configuration on Automate node

### Download and Installation - Generic

```sh
DD_API_KEY= <DataDog Agent API> DD_SITE=<DataDog Site Name> DD_AGENT_MAJOR_VERSION= <DataDog Version> bash -c "$(curl -L curl -L https://s3.amazonaws.com/dd-	agent/scripts/install_script.sh)"
```

Run the below commands with the required details to add a DataDog client on a particular host:

```sh
curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh |DD_AGENT_MAJOR_VERSION= <DataDog Version> DD_API_KEY=<DataDog Agent API> DD_SITE="<DataDog Site Name>" bash -s arg1

sudo systemctl start datadog-agent
```

### Data Dog Installation Setup Steps - Automate HA

* SSH into the bastion machine and go to the deploy workspace

```sh
sudo su -
cd /hab/a2_deploy_workspace
```

* Installation script for data dog:

https://s3.amazonaws.com/dd-agent/scripts/install_script.sh is is the standard script provided by the data dog which is responsible for agent installation.

This script requires:

* Data Dog URL (provide/use data dog URL link here)

  | SITE    | SITE URL                  | SITE PARAMETER    | LOCATION |
  | :------ | :------------------------ | :---------------- | :------- |
  | US1     | https://app.datadoghq.com | datadoghq.com     | US       |
  | US3     | https://us3.datadoghq.com | us3.datadoghq.com | US       |
  | US5     | https://us5.datadoghq.com | us5.datadoghq.com | US       |
  | EU1     | https://app.datadoghq.eu  | datadoghq.eu      | EU       |
  | US1-FED | https://app.ddog-gov.com  | ddog-gov.com      | US       |
  | AP1     | https://ap1.datadoghq.com | ap1.datadoghq.com | Japan    |

* API key (This needs to be generated at the data dog portal, and will require access to the portal)

	* API key details can be taken from the Datadog portal. Listed below are the steps:

		Go to Datadog Portal (https://app.datadoghq.com/)

		Go to: Organization settings -> API Keys

* Modify "tags" in the configuration file:

    * Provide the correct customer name in the "customer" tag. The format should remain the same.
    * Set the “Production” tag as true

* Path to the configuration file for the components where an agent needs to be run

* All the component level configurations as detailed under the [agent configuration section](#agent-configuration)

* Required tags to be added (This is work as a filter while fetching metrics on the data dog console)

### Running the above steps will ensure

* Agent getting installed in each of the nodes (Bastion Machine and All Instances - Chef Server, Automate, Postgres, ElasticSearch nodes)
* **These steps will install the datadog agent in each instance, with the required configuration, and restart the agent.**

Once the datadog agent is up and running, we need to wait for at least 15-20 mins to view the metrics and logs at:
https://app.datadoghq.com/infrastructure

1. Validation on Data Dog console for metrics getting received as per the **tags added**.

### Configuration Types

* Configuration steps can be found here as part of the standard data dog agent config repo: `datadog-agent/cmd/agent at main · DataDog/datadog-agent`

* Configuration YAML files need to be available before running the agent.

* Type of configuration required for Automate HA setup, in order to ensure that all the components of AUtomate HA are sending the relevant metrics to data dog

| Config file (Final DD agent location) | Details   |
| :--- | :-- |
|/etc/datadog-agent/datadog.yaml|**DataDog Main YAML File** Need to change and verify the datadog configuration and tags Reference location for actual values: [DataDog.yaml](YML_Files/datadog.yaml) |
|/etc/datadog-agent/conf.d/http_check.d/conf.yaml |**Automate and Chef Infra Server Metrics - agent config** Automate and Chef Infra Server related application level services health check metrics are captured here as part of HTTP checks. Reference location for actual values: [Automate](YML_Files/automate_httpd.yaml), [Chef Server](YML_Files/chef_server_httpd.yaml) |
|/etc/datadog-agent/conf.d/elastic.d/conf.yaml | **OpenSearch Metrics - agent config** - This config keep information regarding types on OpenSearch metrics that need to be sent to data dog. Reference location for actual values: [Elasticsearch](YML_Files/elastic.yaml) ||
|/etc/datadog-agent/conf.d/postgres.d/conf.yaml|**Postgres Metrics - agent config** This config keeps information regarding types on Postgres metrics that need to be sent to data dog. Reference location for actual values: [Postgres](YML_Files/postgres.yaml) ||
|/etc/datadog-agent/conf.d/disk.d/conf.yaml|**Disk Metrics - All nodes** The Disk check is enabled by default, and the Agent collects metrics on all local partitions. Reference location for actual values: [Disk Metrics](YML_Files/diskd.yaml) |
|/etc/datadog-agent/conf.d/http_check.d/conf.yaml|**Bastion Host Metrics - agent config** Bastion host related application level services health check metrics are captured here as part of HTTP checks. Reference location for actual values: [Bastion](YML_Files/bastion_httpd.yaml)|
|/etc/datadog-agent/conf.d/nginx.d/conf.yaml|**Nginx** - The Datadog Agent can collect many metrics from NGINX instances, including (but not limited to):: Total requests, Connections such as accepted, handled, and active. Reference location for actual values: [Nginx](YML_Files/nginx.yml)|

* For centralized logging metrics send to datadog, pls follow these references configurations :  [DataDog-Centralize_Logs_Management](DataDog-Centralise_Logs_Management.md)

## DataDog Agent Configuration

The Agent v6 configuration file uses YAML to better support complex configurations. Agent support standard OS such as Linus, MacOS, AIX, and Windows.
| PLATFORM | LOCATION  |
| :--- | :-- |
|Linux|/etc/datadog-agent/datadog.yaml|
|Ubuntu|/etc/datadog-agent/datadog.yaml/|
|CentOS|/etc/datadog-agent/datadog.yaml/|

Reference location for actual configurational values: [datadog.yaml](YML_Files/datadog.yaml)

### Steps to create a Datadog user

Datadog agent will read and collects the metrics from all the instances and managed services for monitoring and for that, it needs a user in the Postgres database. Given below are the steps to create the Datadog user in the database:

* SSH into the bastion machine and go to the deploy workspace

```sh
sudo su -
cd /hab/a2_deploy_workspace
```

* Ssh into the automate instance using:

```sh
automate-cluster-ctl ssh automate
```

* Go to the PSQL directory and open the PSQL console:

```sh
sudo su -
cd /hab/pkgs/core/postgresql13-client/13.5/20220120152435/bin
./psql --host=<hostname of RDS> --username=root --dbname=postgres
```

The password for the root user is admin1234

* Run the below commands on PSQL console to create user for datadog:

	* Create user datadog with password 'datadog';
	* Grant SELECT ON pg_stat_database to datadog;
	* Grant pg_monitor to datadog;

* Exit out of the Automate Instance and back to the bastion host.

### Additional steps required for Automate HA deployment with managed services

Update hostname in `postgres.yaml`. Given below are the steps to Update hostname in `postgres.yaml`:

* SSH into the bastion machine and go to the deploy workspace

```sh
sudo su -
cd /hab/a2_deploy_workspace
```

* SSH into the automate instance using:

```sh
automate-cluster-ctl ssh automate
```

* Add/Edit the postgres.yaml file and modify the below-listed details:

```sh
vi automate-backend-datadog/postgresql/postgres.yaml
```

* Modify "host": Provide the hostname of your RDS Postgres instance

* Modify "tags": Provide the correct customer name in the "customer" tag. The format should remain the same.
