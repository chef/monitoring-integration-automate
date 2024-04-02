# Datadog Download, Installation, and Configuration

We also recommend referring to Datadog [documentation](https://docs.datadoghq.com/agent/) on Datadog agent installation and configuration.

## Datadog Download and Installation

To monitor the Chef Automate HA infrastructure, a Datadog monitoring agent needs to run on the infrastructure nodes to get the possible configured metrics and other information that can be sent to the monitoring tool on the server side to track the health of the overall infrastructure.

### Agent Definition

Agents for monitoring use cases refer to software on your host machines. It collects events and metrics from hosts and sends them to the centralized monitoring tool’s 
infrastructure, where we can analyze the monitoring and performance data.

### Use case-wise Requirements

1. Automate HA with On-Premises and AWS deployment. Datadog agent to be configured on:

    * Automate node(s)
    * Chef Infra Server(s)
    * Chef-managed OpenSearch nodes
    * Chef-managed PostgreSQL nodes
    * Bastion node

2. Automate HA with AWS deployment with Managed services

    * Datadog agent to be configured on:
        * Automate node
        * Chef Infra Server
    * AWS account integration with Datadog
    * Postgres YML configuration on Automate node

### Download and Installation - Generic

```sh
DD_API_KEY= <Datadog Agent API> DD_SITE=<Datadog Site Name> DD_AGENT_MAJOR_VERSION= <Datadog Version> bash -c "$(curl -L curl -L https://s3.amazonaws.com/dd-   agent/scripts/install_script.sh)"
```

Run the below commands with the required details to add a Datadog client on a particular host:

```sh
curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh |DD_AGENT_MAJOR_VERSION= <Datadog Version> DD_API_KEY=<Datadog Agent API> DD_SITE="<Datadog Site Name>" bash -s arg1
sudo systemctl start datadog-agent
```

### Datadog Installation Setup Steps - Automate HA

* SSH into the bastion machine and go to the deploy workspace

```sh
sudo su -
cd /hab/a2_deploy_workspace
```

* Installation script for Datadog:

https://s3.amazonaws.com/dd-agent/scripts/install_script.sh is the standard script the Datadog responsible for agent installation provides.

This script requires:

* Datadog URL (provide/use Datadog URL link here)

  | SITE    | SITE URL                  | SITE PARAMETER    | LOCATION |
  | :------ | :------------------------ | :---------------- | :------- |
  | US1     | https://app.datadoghq.com | datadoghq.com     | US       |
  | US3     | https://us3.datadoghq.com | us3.datadoghq.com | US       |
  | US5     | https://us5.datadoghq.com | us5.datadoghq.com | US       |
  | EU1     | https://app.datadoghq.eu  | datadoghq.eu      | EU       |
  | US1-FED | https://app.ddog-gov.com  | ddog-gov.com      | US       |
  | AP1     | https://ap1.datadoghq.com | ap1.datadoghq.com | Japan    |

* API key (This needs to be generated at the Datadog portal, and will require access to the portal)

    * API key details can be taken from the Datadog portal. Listed below are the steps:

        Go to Datadog Portal [https://app.datadoghq.com/](https://app.datadoghq.com/)

        Go to: *Organization settings -> API Keys*

* Modify `tags` in the configuration file:

    * Provide the correct customer name in the `customer` tag. The format should remain the same.

    * Set the `Production` tag as true

* Path to the configuration file for the components where an agent needs to be run

* All the component level configurations as detailed under the [agent configuration section](#agent-configuration)

* Required tags to be added (This works as a filter while fetching metrics on the Datadog console)

### Running the above steps will ensure

* Agent getting installed in each of the nodes (Bastion Machine and All Instances - Chef Server, Automate, PostgreSQL, Elasticsearch nodes)

* **These steps will install the Datadog agent in each instance, with the required configuration, and restart the agent.**

Once the Datadog agent is up and running, we need to wait for at least 15-20 minutes to view the metrics and logs at: [https://app.datadoghq.com/infrastructure](https://app.datadoghq.com/infrastructure)

1. Validation on Datadog console for metrics getting received as per the **tags added**.

### Configuration Types

* Configuration steps can be found here as part of the standard Datadog agent config repo: `datadog-agent/cmd/agent at main · Datadog/datadog-agent`.

* Configuration YAML files need to be available before running the agent.

* Type of configuration required for Automate HA setup to ensure that all the components of AUtomate HA are sending the relevant metrics to Datadog.

| Config file (Final DD agent location) | Details   |
| :--- | :-- |
|/etc/datadog-agent/datadog.yaml|**Datadog Main YAML File** Need to change and verify the Datadog configuration and tags Reference location for actual values: [Datadog.yaml](YML_Files/datadog.yaml) |
|/etc/datadog-agent/conf.d/http_check.d/conf.yaml |**Automate and Chef Infra Server Metrics - agent config** Automate and Chef Infra Server related application level services health check metrics are captured here as part of HTTP checks. Reference location for actual values: [Automate](YML_Files/automate_httpd.yaml), [Chef Server](YML_Files/chef_server_httpd.yaml) |
|/etc/datadog-agent/conf.d/elastic.d/conf.yaml | **OpenSearch Metrics - agent config** - This config keeps information regarding types of OpenSearch metrics that must be sent to the Datadog. Reference location for actual values: [Elasticsearch](YML_Files/elastic.yaml) ||
|/etc/datadog-agent/conf.d/postgres.d/conf.yaml|**PostgreSQL Metrics - agent config** This config keeps information regarding types of PostgreSQL metrics that must be sent to the Datadog. Reference location for actual values: [PostgreSQL](YML_Files/postgres.yaml) ||
|/etc/datadog-agent/conf.d/disk.d/conf.yaml|**Disk Metrics - All nodes** The Disk check is enabled by default, and the Agent collects metrics on all local partitions. Reference location for actual values: [Disk Metrics](YML_Files/diskd.yaml) |
|/etc/datadog-agent/conf.d/http_check.d/conf.yaml|**Bastion Host Metrics - agent config** Bastion host related application level services health check metrics are captured here as part of HTTP checks. Reference location for actual values: [Bastion](YML_Files/bastion_httpd.yaml)|
|/etc/datadog-agent/conf.d/nginx.d/conf.yaml|**Nginx** - The Datadog Agent can collect many metrics from NGINX instances, including (but not limited to):: Total requests and connections such as accepted, handled, and active. Reference location for actual values: [Nginx](YML_Files/nginx.yml)|

* For centralized logging metrics sent to Datadog, please follow these reference configurations: [Datadog-Centralize_Logs_Management](DatadDog-Centralise_Logs_Management.md)

## Datadog Agent Configuration

The Agent v6 configuration file uses YAML to support complex configurations better. Agent supports standard OS such as Linus, MacOS, AIX, and Windows.

| PLATFORM | LOCATION  |
| :--- | :-- |
|Linux|/etc/datadog-agent/datadog.yaml|
|Ubuntu|/etc/datadog-agent/datadog.yaml/|
|CentOS|/etc/datadog-agent/datadog.yaml/|

Reference location for actual configurational values: [datadog.yaml](YML_Files/datadog.yaml)

### Steps to create a Datadog user

Datadog agent will read and collect the metrics from all the instances and managed services for monitoring, and for that, it needs a user in the PostgreSQL database. Given below are the steps to create the Datadog user in the database:

* SSH into the bastion machine and go to the deploy workspace:

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

The password for the root user is *admin1234*

* Run the below commands on the PSQL console to create a user for Datadog:

    * Create user Datadog with password *datadog*;

    * Grant SELECT ON pg_stat_database to Datadog;

    * Grant pg_monitor to Datadog;

* Exit out of the Automate Instance and back to the bastion host.

### Additional steps required for Automate HA deployment with managed services

Update hostname in `postgres.yaml`. Given below are the steps to Update the hostname in `postgres.yaml`:

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
