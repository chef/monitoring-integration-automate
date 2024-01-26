# Monitoring recommendations for Chef Automate HA


Chef Automate HA is a platform that enables continuous automation of infrastructure, compliance, and application delivery. Monitoring of Chef Automate HA solution is a critical component for any organization to ensure the health, resources and service availability to deliver continuous automation of infrastructure, compliance, and applications.

Typically, in a High Availability (HA) setup, you would have multiple instances of various components distributed across different nodes to ensure redundancy and fault tolerance. Common components in a Chef Automate HA setup may include Chef Automate serve(s), Chef Infra Server(s), Postgres database Servers, OpenSearch Serves and Bastion Server.

This document outlines the recommendations for Chef Automate HA solution monitoring for components used in the solution.

## Server Level Metrics:
The following metrics are commanded to monitor on all Chef Automate HA Servers

| **Component**           | **Metrics Description**                        |                             
|-------------------------|------------------------------------------------|  
|CPU Usage                | Percentage of CPU utilization                   |
|CPU Steel                | Percentage of time a virtual CPU within a cloud server involuntarily waits on a physical CPU for its processing time. |
|Memory Usage             | Percentage of memory utilization |
|Swap Usage               | Monitor swap space to ensure that the system is not excessively relying on virtual memory |
|Disk Usage by mount point|Percentage of disk space used |
|System Uptime            | Ensures system availability |

## Chef Automate Server Metrics:
The following metrics are recommended to monitor Chef Automate servers along with metrics defined for server level.

| **Component**           | **Metrics Description**                        |                             
|-------------------------|------------------------------------------------|  
| Automate Services Status | Status of 31 services running on Automate Server. |
| Automate LB 5XX Alert | Generate alerts for chef automate load balanced url for response code 500 or more |
| Chef-Server LB 5XX Alert | Generate alerts for chef server load balanced url for response code 500 or more |

## Chef Infra Server Metrics:
The following metrics are recommended to monitor Chef Infra servers along with metrics defined for server level.

| **Component**           | **Metrics Description**                        |                             
|-------------------------|------------------------------------------------|  
| Infra Services Status | Status of 8 services running on Infra Server. |
| Automate LB 5XX Alert | Generate alerts for chef automate load balanced url for response code 500 or more |
| Chef-Server LB 5XX Alert | Generate alerts for chef server load balanced url for response code 500 or more |

## Postgres Database Server Metrics:
The following metrics are recommended to monitor Postgres database servers along with metrics defined for server level.

| **Component**           | **Metrics Description**                        |                             
|-------------------------|------------------------------------------------|  
| PG Can Connect | Ensures that database is up, running and connect |
| Connection Exhaustion | Major Alert if the total number of database connections goes above 90% of max allowed connections |
| Connection Exhaustion | Major Alert if the total number of database connections goes above 95% of max allowed connections |
| Managed PostgreSQL Write Latency | Alert when write latency increases above 300 |
| Managed PostgreSQL Read Latency | Alert when read latency increases above 300 300 |

## OpenSearch Server Metrics:
The following metrics are recommended to monitor OpenSearch servers along with metrics defined for server level.

| **Component**           | **Metrics Description**                        |                             
|-------------------------|------------------------------------------------|  
| ES Cluster Health Check | Alert when opensearch cluster nodes count drops below  2 |
| ES Heap Usage Factor | Alert when opensearch jvm mem heap used percent exceeds 95 |
| ES Performance Alert | Alert when opensearch index search fetch time seconds exceeds  30 | 
| ES Performance Alert | Alert when opensearch index search fetch time seconds exceeds 60 |
| ES Indexing latency Alert | Alert when opensearch index indexing index time seconds exceeds 500 |
| Elasticsearch Search latency Alert | Alert when opensearch index search query time seconds exceeds 60 |