# Monitoring recommendations for Chef Automate HA


Chef Automate HA is a platform that enables continuous automation of infrastructure, compliance, and application delivery. Monitoring of Chef Automate HA solution is a critical component for any organization to ensure the health, resources and service availability to deliver continuous automation of infrastructure, compliance, and applications.

Typically, in a High Availability (HA) setup, you would have multiple instances of various components distributed across different nodes to ensure redundancy and fault tolerance. Common components in a Chef Automate HA setup may include Chef Automate serve(s), Chef Infra Server(s), Postgres database Servers, OpenSearch Serves and Bastion Server.

This document outlines the recommendations for Chef Automate HA solution monitoring for components used in the solution.

## Server Level Metrics:
The following metrics are commanded to monitor on all Chef Automate HA Servers

    | **Component**           | **Metrics Description**                        |                             
    |-------------------------|------------------------------------------------|  
    |CPU Usage                |Percentage of CPU utilization                   |
    |CPU Steel                |Percentage of time a virtual CPU within a cloud server involuntarily waits on a physical CPU for its processing time.                |
    |Memory Usage             | Percentage of memory utilization               |
    |Swap Usage               | Swap Usage|
    |Disk Usage by mount point|Percentage of disk space used                   |
    |System Uptime            | Ensures system availability                    |

## Chef Automate Server Metrics:
The fallowing metrics are recommended to monitor Chef Automate servers along with metrics defined for server level.

## Chef Infra Server Metrics:
The fallowing metrics are recommended to monitor Chef Infra servers along with metrics defined for server level.

## Postgres Database Server Metrics:
The fallowing metrics are recommended to monitor Postgres database servers along with metrics defined for server level.

## OpenSearch Server Metrics:
The fallowing metrics are recommended to monitor OpenSearch servers along with metrics defined for server level.
