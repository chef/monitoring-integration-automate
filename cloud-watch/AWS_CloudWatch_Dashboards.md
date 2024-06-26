# Automate HA cluster metrics monitoring Dashboard created in Amazon CloudWatch

## Description

As an Automate HA cluster user, you must set up an AWS CloudWatch dashboard to monitor the health of various components of Automate HA, along with the overall health and service-related stats visible in one place.

### AWS CloudWatch Dashboard

* Amazon CloudWatch dashboards are customizable home pages in the CloudWatch console that you can use to monitor your resources in a single view, even those spread across different Regions. You can use CloudWatch dashboards to create customized views of the metrics and alarms for your AWS resources.

* With dashboards, you can create the following:

  * A single view of selected metrics and alarms to help you assess the health of your resources and applications across one or more regions. You can choose the color used for each metric on each graph to track the same metric across multiple graphs easily.

  * An operational playbook that guides team members during operational events about how to respond to specific incidents.

  * A common view of critical resource and application measurements that team members can share for faster communication during operational events.

* Reference Document: [Using Amazon CloudWatch dashboards](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html)

#### AWS CloudWatch dashboard and widget collections are set up on the Automate HA cluster

* System metrics
* Infrastructure Health
* Service level health
* PostgreSQL metrics
* OpenSearch metrics

### Dashboard configuration setup

To get started, create a CloudWatch dashboard. You can create multiple dashboards and add them to a favorites list. You aren't limited to the number of dashboards you can have in your AWS account. All dashboards are global. See the [Create Dashboard](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create_dashboard.html) page for further reference.

1. Open the CloudWatch console.

1. In the navigation pane, choose **Dashboards**, and then choose **Create Dashboard**.

1. In the Create new dashboard dialog box, enter a name for the dashboard, and then choose Create dashboard.

1. Select a source - *Metrics* or *Logs* on which the dashboard widget to create.

   ![CloudWatch_Dashboard_widgetSource](images/CloudWatch_Dashboard_widgetSource.png)

1. Choose the metrics graph for the Metrics source dashboard selected in step 4.

   ![CloudWatch_Dashboard_selectmetrics](images/CloudWatch_Dashboard_selectmetrics.png)

1. Choose **Add widget**, then repeat step 4 to add another widget to the dashboard. You can repeat this step multiple times.

   ![CloudWatch_Dashboard_WidgetCreate](images/CloudWatch_Dashboard_WidgetCreate.png)

1. Choose **Save dashboard**.

   ![CloudWatch_Dashboard_Save](images/CloudWatch_Dashboard_Save.png)

Refer [here](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create_dashboard.html) for more details.

**Note:** Refer to [Logs and metrics supported by Amazon CloudWatch Application Insights](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/appinsights-logs-and-metrics.html) for a list of metrics that can be configured.

### Automatic Amazon CloudWatch Dashboards

AWS CloudWatch provides a dashboard for resources that are already available.

![CloudWatch_AutomaticDashboard_List](images/CloudWatch_AutomaticDashborad_list.png)

![CloudWatch_AutomaticDashboard_Sample](images/CloudWatch_AutomaticDashboard_Sample.png)

Using the steps explained above, we can use the above dashboard or create more inclined to automate the HA cluster as needed.
