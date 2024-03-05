# Monitoring recommendations for Chef Automate HA


Chef Automate HA is a platform that enables continuous automation of infrastructure, compliance, and application delivery. Monitoring of Chef Automate HA solution is a critical component for any organization to ensure the health, resources and service availability to deliver continuous automation of infrastructure, compliance, and applications.

Typically, in a High Availability (HA) setup, you would have multiple instances of various components distributed across different nodes to ensure redundancy and fault tolerance. Common components in a Chef Automate HA setup may include Chef Automate Server(s), Chef Infra Server(s), Postgres database Servers, OpenSearch Servers and Bastion Server.

This document outlines the recommendations for Chef Automate HA solution monitoring for components used in the solution.

## Server Level Metrics:
The following metrics are recommended to monitor on all Chef Automate HA Servers

| **Component**           | **Metrics Description**                        |  **Severity Level**     | **Trigger After**  |  **Alert Type**  |                       
|-------------------------|------------------------------------------------|-------------------|-------------------|-------------------|  
|CPU Usage                | Percentage of CPU utilization                   |L1 | 10 Minutes | PagerDuty |
|CPU Steel                | Percentage of time a virtual CPU within a cloud server involuntarily waits on a physical CPU for its processing time. |L1 | 10 Minutes | PagerDuty |
|Memory Usage             | Percentage of memory utilization exceeds above 95% | L2 | 10 Minutes | Slack |
|Swap Usage               | Monitor swap space to ensure that the system is not excessively relying on virtual memory ||||
|Disk Usage by mount point|Percentage of disk space used exceeds 90%|L1 | 10 Minutes | PagerDuty |
|System Uptime            | Ensures system availability |L1 | 5 Minutes | PagerDuty |

## Chef Automate Server Metrics:
The following metrics are recommended to monitor Chef Automate servers along with metrics defined for server level.

| **Component**           | **Metrics Description**                        |  **Severity Level**     | **Trigger After**  |  **Alert Type**  |                       
|-------------------------|------------------------------------------------|-------------------|-------------------|-------------------| 
| Automate Services Status | Health status of 31 services running on Automate Server. e.g. http://localhost:9631/services/"named-service"/default/health. Replace the named-service with actual automate service. |L1 | 5 Minutes | PagerDuty |
| Automate LB 5XX Alert | Generate alerts for chef automate load balanced url for response code 500 or more |L1 | 10 Minutes | PagerDuty |
| Chef-Server LB 5XX Alert | Generate alerts for chef server load balanced url for response code 500 or more |L1 | 10 Minutes | PagerDuty |

## Chef Infra Server Metrics:
The following metrics are recommended to monitor Chef Infra servers along with metrics defined for server level.

| **Component**           | **Metrics Description**                        |  **Severity Level**     | **Trigger After**  |  **Alert Type**  |                       
|-------------------------|------------------------------------------------|-------------------|-------------------|-------------------|  
| Infra Services Status | Health status of 8 services running on Infra Server. e.g. http://localhost:9631/services/"named-service"/default/health. Replace the named-service with actual infra service. |L1 | 5 Minutes | PagerDuty |
| Automate LB 5XX Alert | Generate alerts for chef automate load balanced url for response code 500 or more |L1 | 10 Minutes | PagerDuty |
| Chef-Server LB 5XX Alert | Generate alerts for chef server load balanced url for response code 500 or more |L1 | 10 Minutes | PagerDuty |

## Postgres Database Server Metrics:
The following metrics are recommended to monitor Postgres database servers along with metrics defined for server level.

| **Component**           | **Metrics Description**                        |  **Severity Level**     | **Trigger After**  |  **Alert Type**  |                       
|-------------------------|------------------------------------------------|-------------------|-------------------|-------------------| 
| PG Can Connect | Ensures that database is up, running and connect | L1 | 1 Minute | PagerDuty |
| Connection Exhaustion | Major Alert if the total number of database connections goes above 90% of max allowed connections | L2 | 10 Minutes | Slack |
| Connection Exhaustion | Major Alert if the total number of database connections goes above 95% of max allowed connections | L1 | 5 Minutes | PagerDuty |
| Managed PostgreSQL Write Latency | Alert when write latency exceeds above 300 |L2 | 10 Minutes | Slack |
| Managed PostgreSQL Read Latency | Alert when read latency exceeds above 300 |L2 | 10 Minutes | Slack |

## OpenSearch Server Metrics:
The following metrics are recommended to monitor OpenSearch servers along with metrics defined for server level.

| **Component**           | **Metrics Description**                        |  **Severity Level**     | **Trigger After**  |  **Alert Type**  |                       
|-------------------------|------------------------------------------------|-------------------|-------------------|-------------------|
| ES Cluster Health Check | Alert when opensearch cluster nodes count drops below  2 | L1 | 5 Minutes | PagerDuty |
| ES Heap Usage Factor | Alert when opensearch jvm mem heap used percent exceeds 95 | L1 | 5 Minutes | PagerDuty |
| ES Performance Alert | Alert when opensearch index search fetch time seconds exceeds  30 | L2 | 10 Minutes | Slack |
| ES Performance Alert | Alert when opensearch index search fetch time seconds exceeds 60 | L1 | 10 Minutes | PagerDuty |
| ES Indexing latency Alert | Alert when opensearch index indexing index time seconds exceeds 500 | L1 | 15 Minutes | PagerDuty |
| Elasticsearch Search latency Alert | Alert when opensearch index search query time seconds exceeds 60 | L1 | 15 Minutes | PagerDuty |