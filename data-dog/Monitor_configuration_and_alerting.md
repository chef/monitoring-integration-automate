# Monitor Configuration and Alerting

## Description

As an Automate HA customer, I need to set monitoring rules for the available metrics on the Datadog console, which can be used to track and notify me about Automate HA infrastructure-related health.

A metric monitor provides alerts and notifications if a specific metric is above or below a certain threshold.

## Datadog Monitors

* Monitors are based on metrics collected by the Datadog agent. Create monitors that warn or alert based on the count of any metrics across hosts or tags.

* Metric monitors are helpful for a continuous stream of metrics. Any metric sent to Datadog can be alerted upon if it crosses a threshold over a given period.

* Here is the list of monitoring rules and logical conditions and details to be referenced for monitor creation: [Monitoring rule list](Monitoring_rule_list.md).

## Prerequisites

Before getting started, you need a Datadog account linked to a host with the Datadog Agent installed.

### Steps to Perform at the Datadog UI

1. To create a metric monitor in Datadog, use the main navigation: *Monitor -> new monitor* and create a Custom Monitor.

1. To define the metric - There are options to create monitors based on Hosts. Process, metrics, etc. For this use case, we are going to create a monitor for Hosts.

      ![Data_Dog_Metrics](Images/Data_Dog_Metrices.png)

1. Pick hosts you want to monitor, filtering those by tags that need to be added to the hosts at the time of creation.

      ![Metrics_Host_Monitor](Images/Metrics_Host_Monitor.png)

1. **Set alert conditions:** Select the option to automatically resolve the alerts after a specified period or to require manual intervention by a user.

1. **Notify your team:** You can notify the team responsible for the infrastructure monitoring via email, Slack, Teams, or PagerDuty. Devoted sections explain the detailed steps for integrating Datadog with these alerting applications.

1. Datadog provides the flexibility of notification of the same alert, which can be configured to renotify in the alerting app multiple times after a fixed interval of time.

      ![Metrics_Host_Monitor](Images/Notify.png)

1. Permissions: Different alerts need to be assigned a priority level depending on their impact on the applications' performance or functioning. Priority should be set for custom alerts to address those that meet the SLAs.

1. Role-based access control needs to be applied to the custom monitors and alerts created, which dictates who can edit/delete my alert and to whom it will be notified.

      ![Metrics_Host_Monitor](Images/Priority.png)

1. Select the **Save** button.

You can now view Monitored saved at the **Manage Monitors** tab and edit and trigger it as needed.
