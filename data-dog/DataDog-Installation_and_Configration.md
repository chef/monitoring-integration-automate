# DataDog Installation and Configration
**Description**

As an Automate HA customer, need to have a monitoring agent running on the infrastructure nodes to get the possible configured metrics and other information which can be sent to the monitoring tool in order to track the health of the overall infrastructure.

**Agent definition:**

Agents for monitoring use cases refer to software that runs on your host machines. It collects events and metrics from hosts and sends them to the centralized monitoring tool’s infrastructure, where we can analyze the monitoring and performance data.

**Installing DataDog Agent**

DD_API_KEY= <DataDog Agent API> DD_SITE=<DataDog Site Name> DD_AGENT_MAJOR_VERSION= <DataDog Version> bash -c "$(curl -L curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"

	curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh |DD_AGENT_MAJOR_VERSION= <DataDog Version> DD_API_KEY=<DataDog Agent API> DD_SITE="<DataDog Site Name>" bash -s arg1

	sudo systemctl start datadog-agent

**Data dog installation setup steps (Introductory reference):**
Installation script for data dog:

	https://s3.amazonaws.com/dd-agent/scripts/install_script.sh

This is the standard script provided by the data dog which is responsible for agent installation.

This script requires:
+ Data Dog URL (provide/use data dog URL link here)

  | SITE    | SITE URL                  | SITE PARAMETER    | LOCATION |
  | :------ | :------------------------ | :---------------- | :------- |
  | US1     | https://app.datadoghq.com | datadoghq.com     | US       |
  | US3     | https://us3.datadoghq.com | us3.datadoghq.com | US       |
  | US5     | https://us5.datadoghq.com | us5.datadoghq.com | US       |
  | EU1     | https://app.datadoghq.eu  | datadoghq.eu      | EU       |
  | US1-FED | https://app.ddog-gov.com  | ddog-gov.com      | US       |
  | AP1     | https://ap1.datadoghq.com | ap1.datadoghq.com | Japan    |

+ API key (This needs to be generated at the data dog portal, and will require access to the portal)

  Get the API at Organization Settings -> API Keys

+ Path to the configuration file for the components where an agent needs to be run
+ All the component level configurations as detailed under the agent configuration section
+ List of IPs where we want to run the agent per configuration (this is to automate the installation on all nodes at once)
+ Required tags to be added (This is work as filter while fetching metrics on data dog console)

**Running this script will ensure**
+ Agent getting installed in each of the nodes (Bastion Machine and All Instances - Chef Server, Automate, Postgress, ElasticSearch nodes)
+ Enabling the component-level configuration as per the setup.


**Acceptance Criteria:**

1. Data dog agent to be configured for:
   + Automate node
   + Chef Infra Server
   + Chef managed OpenSearch
   + Chef managed Postgres
   + Bastion node

2. Data dog agent to be running on Automate,
chef infra server nodes, OpenSearch, and Postgres nodes.

1. Validation on Data Dog console for metrics getting received as per the **tags added**.

**Config types:**
+ Configuration steps can be found here as part of the standard data dog agent config repo:

		datadog-agent/cmd/agent at main · DataDog/datadog-agent

+ Configuration YAML files need to be available before running the agent.

+ Type of configuration required for Automate HA setup, in order to ensure that all the components of AUtomate HA are sending the relevant metrics to data dog

| Config file (Final DD agent location) | Details   |
| :--- | :-- |
|/etc/datadog-agent/conf.d/http_check.d/conf.yaml |**Automate and Chef Infra Server Metrics - agent config** Automate and Chef Infra Server related application level services health check metrics are captured here as part of HTTP checks. Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/automate/httpd.yaml https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/chef_server/httpd.yaml |
|/etc/datadog-agent/conf.d/elastic.d/conf.yaml | **OpenSearch Metrics - agent config** - This config keep information regarding types on OpenSearch metrics which needs to be sent to data dog. Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/elasticsearch/elastic.yaml |
|/etc/datadog-agent/conf.d/postgres.d/conf.yaml|**Postgres Metrics - agent config** This config keep information regarding types on Postgres metrics which needs to be sent to data dog. Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/postgresql/postgres.yaml|
|/etc/datadog-agent/conf.d/disk.d/conf.yaml|**Disk Metrics - All nodes** The Disk check is enabled by default, and the Agent collects metrics on all local partitions. Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/chef_server/diskd.yaml|

**Agent configuration:**

The Agent v6 configuration file uses YAML to better support complex configurations. Agent support standard OS such as Linus, MacOS, AIX, and Windows.
| PLATFORM | COMMAND   |
| :--- | :-- |
|AIX|/etc/datadog-agent/datadog.yaml|
|Linux|/etc/datadog-agent/datadog.yaml|
|macOS|~/.datadog-agent/datadog.yaml|
|Windows Server 2008, Vista and newer|%ProgramData%\Datadog\datadog.yaml|

Reference location for actual values: ReferenceFiles/datadog.yml