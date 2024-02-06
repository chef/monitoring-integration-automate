# Monitoring, Alerting, and Centralised Logging integration support with Chef Automate HA
The Chef Automate HA equates to reliability, efficiency, and productivity, built on Redundancy and Failover. It aids in addressing significant issues like service failure and zone failure. Please refer to the public [documentation](https://docs.chef.io/automate/ha/) of Automate HA for more information.

This document provides guided steps on how to build and integrate Monitoring, Alerting, and Centralised logging tools with Chef Automate HA. Based on our analysis we have selected a few tools which is our recommendation. 

## Abstract:
### Monitoring Recommendations

### Tools for Monitoring:
1. Data Dog

2. Prometheus 

3. AWS CloudWatch


### Tools for Alerting:
1. Pager Duty

2. Slack

3. Microsoft Teams


### Tools for Centralised Logging:
1. ELK (Elasticsearch, Logstash, and Kibana)

2. Data Dog 

3. AWS CloudWatch


## Introduction:

The Chef engineering team has comprehensively documented the [recommended monitoring metrics](./Monitoring_Recommendations.md) to offer visibility into the operational health of Chef Automate HA solution. 

As part of the guided steps of integration for the above-mentioned tools, we will capture the below use cases from an integration perspective:

**Agent download and configuration**

This use case covers the steps to download and configure the tools agent which will be running on the nodes(of the Automate HA infrastructure) and will be responsible for scraping the metrics and logs from that nodes. This section also covers the type of configurations that needs to be stepped in order to scrap various kind of component-level metrics.

**Agent Installation**

This use case covers the steps to install the agent and any other extra setup which is required in order to ensure the metrices and logs are covered from each nodes and a component of Automate HA.

**Server setup and configuration**

This use case covers the steps of server setup installation and configuration recommendations. 

**Dashboard Setup and Configuration**

This use case covers the list of recommended dashboards and how to set them up based of various tools steps. This also covers the various configuration aspects which is required for bringing up the dashboard.

**Metrics Configuration and Monitoring Rules Setup**

This use case covers the list of recommended metrics for Automate HA system and various levels of recommended rules to be applied to creating the monitoring based on these metrics. These are just the recommendations only and based on organizational requirements they can add more rules, update on these rules, and alerting mechanisms as required.

**Slack Integration with the tool**

This use case covers permissions and configuration required for allowing Slack to connect with the tool. This also covers the step-wise setup of alerting groups/channels under monitoring rules in order to receive alerts based on the threshold logic.


**Pager Duty Integration with the tool**

This use case covers permissions and configuration required for allowing Slack to connect with the tool. This also covers the step-wise setup of alerting groups/channels under monitoring rules in order to receive alerts based on the threshold logic.


## Datadog integration with Automate HA - Monitoring

a. [Datadog Agent Configuration and Installation for Chef managed nodes](data-dog/DataDog-Installation_and_Configration.md)

b. [Datadog Metrics configuration and Integration with AWS for AWS Managed services](data-dog/DataDog-AWS_Integration.md)

c. [Metrics Monitor Configuration and Monitoring Rules Setup](data-dog/Monitor_configuration_and_alerting.md)

d. [Dashboard Setup and Configuration](data-dog/DataDog-Dashboard-Setup-and-Configuration.md)


## Datadog integration with Automate HA - Alerting

a. [Slack Integration](data-dog/Slack_Integration_and_notification.md)

b. [PagerDuty Integration](data-dog/PagerDuty_Integration_and_Notification.md)

c. [MS Teams Integration](data-dog/DataDog-MSTeams-Integration-Alerting.md)


## Datadog integration with Automate HA - Centralised Logging

a. [Datadog Centralised Logs Management](data-dog/DataDog-Centralise_Logs_Management.md)



## Prometheus integration with Automate HA - Monitoring

a. [Prometheus Server Configuration and Installation](prometheus/Prometheus-server-installation-and-configuration-setup.md) 

b. [Prometheus Agent Configuration and Installation](prometheus/Prometheus-agent-installation-configuration.md)

c. [Prometheus Metrics and AlertManager configuration](prometheus/Prometheus_Monitor_configuration_and_alerting.md)

d. [Dashboard Setup and Configuration](prometheus/Prometheus-Dashboard-Setup-and-Configuration.md)



## Prometheus integration with Automate HA - Alerting

a. [Slack Integration](prometheus/prometheus_slack_Integration_and_Notification.md)

b. [PagerDuty Integration](prometheus/prometheus_PagerDuty_Integration_and_Notification.md)

c. [MS Teams Integration](prometheus/prometheus_msteams_Integration_and_Notification.md)


### ELK integration with Automate HA - Centralised Logging

a. [ELK - Configuration and Installation](ELK/ELK-installation-and-configuration.md)

b. [ELK Agent - Filebeat Configuration, Installation, and Logging](ELK/filebeat-installation.md)


### Cloudwatch integration with Automate HA - Monitoring 

a. [Metrics Monitor Configuration and Monitoring Rules Setup](cloud-watch/AWS_CloudWatch_Metrics_Monitoring_Configration.md)

b. [Dashboard Setup and Configuration](https://github.com/chef/monitoring-integration-automate/blob/main/cloud-watch/AWS_CloudWatch_Dashboards.md)


### Cloudwatch integration with Automate HA - Alerting

a. [Slack Integration](https://github.com/chef/monitoring-integration-automate/blob/main/cloud-watch/AWS_CloudWatch_Slack_Integration.md)

b. [PagerDuty Integration](https://github.com/chef/monitoring-integration-automate/blob/main/cloud-watch/AWS_CloudWatch_PagerDuty_Integration_Alerting.md)


### Cloudwatch integration with Automate HA - Centralised Logging

a. [AWS Cloudwatch Centralised Logs Management](https://github.com/chef/monitoring-integration-automate/blob/main/cloud-watch/AWS_CloudWatch_Logging_Configration.md)










