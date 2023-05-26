README

**Description**

As an Automate HA customer, I need to have a monitoring agent running on the infrastructure nodes to scrap the possible configured metrics and other information which can be sent to the monitoring tool in order to track the health of the overall infrastructure.

**Agent definition:**

Agents for monitoring use cases refer to software that runs on your host machines. It collects events and metrics from hosts and sends them to the centralized monitoring tool’s infrastructure, where we can analyze the monitoring and performance data.

**Agent configuration:**
The Agent v6 configuration file uses YAML to better support complex configurations. Agent support standard OS such as Linus, MacOS, AIX, and Windows.
PLATFORM
COMMAND
AIX
/etc/datadog-agent/datadog.yaml
Linux
/etc/datadog-agent/datadog.yaml
macOS
~/.datadog-agent/datadog.yaml
Windows Server 2008, Vista and newer
%ProgramData%\Datadog\datadog.yaml

Config types:
Configuration steps can be found here as part of the standard data dog agent config repo:
datadog-agent/cmd/agent at main · DataDog/datadog-agent
Configuration YAML files need to be available 1st before running the agent.
Type of configuration required for Automate HA setup, in order to ensure that all the components of AUtomate HA are sending the relevant metrics to data dog
Config file (Final DD agent location)
Details
/etc/datadog-agent/conf.d/elastic.d/conf.yaml
OpenSearch Metrics - agent config
This config keep information regarding types on OpenSearch metrics which needs to be sent to data dog.
Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/elasticsearch/elastic.yaml
/etc/datadog-agent/conf.d/postgres.d/conf.yaml
Postgres Metrics - agent config
This config keep information regarding types on Postgres metrics which needs to be sent to data dog.
Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/postgresql/postgres.yaml

/etc/datadog-agent/conf.d/http_check.d/conf.yaml
Automate and Chef Infra Server Metrics - agent config
Automate and Chef Infra Server related application level services health check metrics are captured here as part of HTTP checks.
Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/automate/httpd.yaml
https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/chef_server/httpd.yaml
/etc/datadog-agent/conf.d/disk.d/conf.yaml
Disk Metrics - All nodes
The Disk check is enabled by default, and the Agent collects metrics on all local partitions.
Reference location for actual values:
https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/chef_server/diskd.yaml
/etc/datadog-agent/conf.d/journald.d/syslog.yaml
Journalctl logs
In case we want to send the logs as well to data dog, this cionfiguration needs to be added to esnure all journal ctl logs are getting sent to data dog.
Reference location for actual values:  https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/chef_server/journald.yaml
/etc/datadog-agent/conf.d/systemd.d/conf.yaml
System logs
Enable this config for system level logs
Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/chef_server/syslog.yaml
/etc/datadog-agent/conf.d/systemd.d/conf.yaml
Systemd logs
Enable this config for systemd logs
Reference location for actual values: https://github.com/chef/chef-saas/blob/main/automate-backend-datadog/chef_server/systemd.yaml
