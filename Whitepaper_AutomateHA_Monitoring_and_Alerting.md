# Monitoring, Alerting, and Centralised Logging integration support with Chef Automate HA

The Chef Automate HA equates to reliability, efficiency, and productivity, built on Redundancy and Failover. It aids in addressing significant issues like service failure and zone failure. Please refer to the public [documentation](https://docs.chef.io/automate/ha/) of Automate HA for more information.

This document provides guided steps on how to build and integrate Monitoring, Alerting, and Centralized logging tools with Chef Automate HA. Based on our analysis we have selected a few tools which is our recommendation.

## Abstract:

### Monitoring Recommendations

### Tools for Monitoring

1. Datadog

1. Prometheus

1. AWS CloudWatch

### Tools for Alerting

1. Pager Duty

1. Slack

1. Microsoft Teams

### Tools for Centralised Logging

1. ELK (Elasticsearch, Logstash, and Kibana)

1. Datadog

1. AWS CloudWatch

## Introduction

The Chef engineering team has comprehensively documented the [recommended monitoring metrics](./Monitoring_Recommendations.md) to offer visibility into the operational health of the Chef Automate HA solution.

As part of the guided steps of integration for the above-mentioned tools, we will capture the below use cases from an integration perspective:

### Agent download and configuration

This use case covers the steps to download and configure the tools agent which will be running on the nodes(of the Automate HA infrastructure) and will be responsible for scraping the metrics and logs from those nodes. This section also covers the type of configurations that need to be stepped to scrap various kinds of component-level metrics.

### Agent Installation

This use case covers the steps to install the agent and any other extra setup that is required to ensure the metrics and logs are covered from each node and a component of Automate HA.

### Server setup and configuration

This use case covers the steps of server setup installation and configuration recommendations.

### Dashboard Setup and Configuration

This use case covers the list of recommended dashboards and how to set them up based on various tools and steps. This also covers the various configuration aspects that  are required for bringing up the dashboard.

### Metrics Configuration and Monitoring Rules Setup

This use case covers the list of recommended metrics for the Automate HA system and various levels of recommended rules to be applied to creating the monitoring based on these metrics. These are just the recommendations only and based on organizational requirements they can add more rules, update these rules, and alerting mechanisms as required.

### Slack Integration with the tool

This use case covers permissions and configuration required for allowing Slack to connect with the tool. This also covers the step-wise setup of alerting groups/channels under monitoring rules to receive alerts based on the threshold logic.

### Pager Duty Integration with the tool

This use case covers permissions and configuration required for allowing Slack to connect with the tool. This also covers the step-wise setup of alerting groups/channels under monitoring rules to receive alerts based on the threshold logic.

## Datadog integration with Automate HA - Monitoring

1. [Datadog Agent Configuration and Installation for Chef Managed nodes](/data-dog/DataDog-Installation_and_Configration.md)

1. [Datadog Metrics configuration and Integration with AWS for AWS Managed services](/data-dog/DataDog-AWS_Integration.md)

1. [Metrics Monitor Configuration and Monitoring Rules Setup](/data-dog/Monitor_configuration_and_alerting.md)

1. [Dashboard Setup and Configuration](/data-dog/DataDog-Dashboard-Setup-and-Configuration.md)

## Datadog integration with Automate HA - Alerting

1. [Slack Integration](data-dog/Slack_Integration_and_notification.md)

1. [PagerDuty Integration](data-dog/PagerDuty_Integration_and_Notification.md)

1. [MS Teams Integration](data-dog/DataDog-MSTeams-Integration-Alerting.md)

## Datadog integration with Automate HA - Centralized Logging

[Datadog Centralized Logs Management](data-dog/DataDog-Centralise_Logs_Management.md)

## Prometheus integration with Automate HA - Monitoring

1. [Prometheus Server Configuration and Installation](prometheus/Prometheus-server-installation-and-configuration-setup.md)

1. [Prometheus Agent Configuration and Installation](prometheus/Prometheus-agent-installation-configuration.md)

1. [Prometheus Metrics and Alertmanager configuration](prometheus/Prometheus_Monitor_configuration_and_alerting.md)

1. [Dashboard Setup and Configuration](prometheus/Prometheus-Dashboard-Setup-and-Configuration.md)

## Prometheus integration with Automate HA - Alerting

1. [Slack Integration](prometheus/prometheus_slack_Integration_and_Notification.md)

1. [PagerDuty Integration](prometheus/prometheus_PagerDuty_Integration_and_Notification.md)

1. [MS Teams Integration](prometheus/prometheus_msteams_Integration_and_Notification.md)

### ELK integration with Automate HA - Centralized Logging

1. [ELK - Configuration and Installation](ELK/ELK-installation-and-configuration.md)

1. [ELK Agent - Filebeat Configuration, Installation, and Logging](ELK/filebeat-installation.md)

### CloudWatch integration with Automate HA - Monitoring

1. [Metrics Monitor Configuration and Monitoring Rules Setup](cloud-watch/AWS_CloudWatch_Metrics_Monitoring_Configration.md)

1. [Dashboard Setup and Configuration](https://github.com/chef/monitoring-integration-automate/blob/main/cloud-watch/AWS_CloudWatch_Dashboards.md)

### CloudWatch integration with Automate HA - Alerting

1. [Slack Integration](https://github.com/chef/monitoring-integration-automate/blob/main/cloud-watch/AWS_CloudWatch_Slack_Integration.md)

1. [PagerDuty Integration](https://github.com/chef/monitoring-integration-automate/blob/main/cloud-watch/AWS_CloudWatch_PagerDuty_Integration_Alerting.md)

### CloudWatch integration with Automate HA - Centralized Logging

1. [AWS CloudWatch Centralized Logs Management](https://github.com/chef/monitoring-integration-automate/blob/main/cloud-watch/AWS_CloudWatch_Logging_Configration.md)
