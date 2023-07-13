# Prometheus Dashboards
Grafana is used as dashboard tool for Prometheus.

The dashboard allows us to analyze data from across our entire system in a single pane of glass. It enables our team to immediately benefit from dynamic views with no query language or coding required.
 
## Installing and Configuring Grafana: 
This article provides step by step guidance to [install Grafana server locally](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/).

After the successful installation, configure the data source for prometheus. The follow article provides the step by step instructions to [data source configuration](https://prometheus.io/docs/visualization/grafana/#creating-a-prometheus-data-source) configuration.

## Considerations for creating Prometheus Dashboards

**Define your goals:**
You need to have clarity what you want to show and monitor in your dashboard and your audience; e.g., L1 and L2 operations team will require a different set of dashboards versus somebody at the management who wants overall summary on the infrastructure footprint

**Choose the right metrics:**
Not all metrics are created equal, some are of more importance than others. Choose the metrics that are important to you, we have listed down all the important metrics that is critical to your Automate Infrastructure. 

This [Reference Metrics](./Prometheus_Reference_Metrics_List.md) provides you a guidance to monitor Automate HA in Chef Managed Deployment style. You can use this guidance to build metrics and dashboards for other deployment styles

**Use the right visualizations:**
There are a variety of visualizations available in Prometheus. Choose the visualizations that are best suited for the metrics you are tracking. For example, if you are tracking CPU usage, you might use a line chart. If you are tracking errors, you might use a heatmap.

**Add context:**
Don't just show your metrics. Add context to your dashboard by adding labels, descriptions, and annotations. This will help you and your team understand what the metrics mean and why they are important.

**Ease of use:**
Your dashboard should be easy to use. Make sure the layout is clear and the visualizations are easy to understand. You can also add filters and alerts to make it even easier to use your dashboard.

## Types of dashboards to consider

**Screenboards:** Screenboards are designed for adhoc analysis and troubleshooting. They allow you to create a custom layout of widgets and panels to visualize your data.

**Timeboards:** Timeboards are designed for monitoring and alerting. They show a historical view of your data over time, and can be used to set alerts and notifications. 

# Chef Automate HA Recommended Dashboards

Chef recommends the following dashboards for Chef Automate HA implementations:

+ Component Health Dashboard
+ Opensearch Dashboard 
+ Postgresql Dashboard
+ System Dashboard

# Dashboards in Grafana
 Once dashboard objective are defined, dashboard can either be created or imported if already exist on the community.

## Creating a Dashboard

The article provides detailed guidance to [create dashboard in Grafana with prometheus](https://grafana.com/docs/grafana/latest/dashboards/build-dashboards/create-dashboard/).

This section will list down the steps to create a dashboard for Chef Automate HA component health dashboard. This dashboard will provide the :
 * status of load balancers for chef automate and chef infra servers in row 1
 * status of all nodes for chef automate and chef infra in row 2
 * status of all services within each of the chef automate nodes in row 3
 * status of all services within each of the chef infra nodes in row 3
 
 Prerequisites:
 * All chef automate and chef infra server nodes must be configured with the blackbox exporter. Refer to the [agent installation documentation](./Prometheus-agent-installation-configuration.md) for blackbox exporter setup.

 * Configure Prometheus server with required jobs to collect required metrics to feed this dashboard. Refer to [Prometheus.yml file](./prometheus.yml) for the detailed job configurations.

+ Login to Grafana portal http://<< ip >>:3000) with your credentials.
+ On the left hand side, click on Dashboards --> New Dashboard. You will be directed to a new Dashboard screen.
![New Dashboard](./images/dashboard1.png)
+ Add 4 Rows and modify the title
  - Row 1 : Chef Automate Load Balancer Status
  - Row 2 : Chef Frontend Server Status
  - Row 3 : Chef Automate Services Status by nodes
  - Row 4 : Chef Infra Services Status by nodes

+ Add Visualization 
  - Title : Chef Automate Load Balancer
  - Query : probe_http_status_code{job="chef-automate-url"}
  - Visualization Type : Stat
  - Stat styles -> Graph mode : None
  - Color Scheme : From thresholds (by value)
  - value Mappings:
  ![chef automate value mapping](./images/lb_vm.png)
  - Move the newly created visualization under row 1 : Chef Automate Load Balancer Status

+ Add Visualization 
  - Title : Chef Server Load Balancer
  - Query : probe_http_status_code{job="chef-server-url"}
  - Visualization Type : Stat
  - Stat styles -> Graph mode : None
  - Color Scheme : From thresholds (by value)
  - value Mappings:
  ![chef automate value mapping](./images/lb_vm.png)
  - Move the newly created visualization under row 1 : Chef Automate Load Balancer Status

+ Add Visualization 
  - Title : Chef Automate Nodes
  - Query : avg(probe_http_status_code{job=~"automate-services-.*"}) by (job)
  - Transform :   
    Match : {job="(\w.*)"}  
    Replace :  $1
  - Visualization Type : Stat
  - Stat styles -> Graph mode : None
  - Stat styles -> Text alignment : Center
  - Color Scheme : From thresholds (by value)
  - value Mappings:
  [chef automate value mapping](./images/chef_services_vm.png)
  - Move the newly created visualization under row 2 : Chef Frontend Server Status

+ Add Visualization 
  - Title : Chef Infra Nodes
  - Query : avg(probe_http_status_code{job=~"chef-server-services-.*"}) by (job)
  - Transform :   
    Match : {job="(\w.*)"}  
    Replace :  $1
  - Visualization Type : Stat
  - Stat styles -> Graph mode : None
  - Stat styles -> Text alignment : Center
  - Color Scheme : From thresholds (by value)
  - value Mappings:
  ![chef automate value mapping](./images/chef_services_vm.png)
  - Move the newly created visualization under row 2 : Chef Frontend Server Status

+ Add Visualization 
    -  Repeat the following steps for each Automate node and update title, query and transform match for each automate node.
  - Title : Chef Automate - Node 1
  - Query : probe_http_status_code{job=~"automate-services-node-01"}
  - Transform :   
    Match : probe_http_status_code{instance="http:\/\/localhost:9631\/services\/(\w.*)\/default\/health", job="automate-services-node-01"}  
    Replace :  $1
  - Visualization Type : Stat
  - Stat styles -> Graph mode : None
  - Stat styles -> Text alignment : Center
  - Color Scheme : From thresholds (by value)
  - value Mappings:
  ![chef automate value mapping](./images/chef_services_vm.png)
  - Move the newly created visualization(s) under row 3 : Chef Automate Services Status by nodes

+ Add Visualization 
  - Repeat the following steps for each Chef Infra node and update title, query and transform match for each Chef Infra node.
  - Title : Chef Infra - Node 1
  - Query : probe_http_status_code{job=~"chef-server-services-node-01"}
  - Transform :   
    Match : probe_http_status_code{instance="http:\/\/localhost:9631\/services\/(\w+-\w.*)\/default\/health", job="chef-server-services-node-01"}    
    Replace :  $1  
  - Visualization Type : Stat  
  - Stat styles -> Graph mode : None
  - Stat styles -> Text alignment : Center
  - Color Scheme : From thresholds (by value)
  - value Mappings:
  ![chef automate value mapping](./images/chef_services_vm.png)
  - Move the newly created visualization(s) under row 4 : Chef Infra Services Status by nodes

+ Save the dashboard.  

+ A json copy of this dashboard can be found [here](./dashboards/Chef%20Automate%20HA%20-%20Component%20Health-1688729683107.json).

+ Please make necessary job configuration changes to make this dashboard work your environment.

## Importing a Dashboard

The prometheus community has contributed the various dashboards and the following dashboards may be configured to monitor Chef Automate HA implementation. 

* [System Dashboard](https://grafana.com/grafana/dashboards/1860-node-exporter-full/)
* [Postgres Dashboard](https://grafana.com/grafana/dashboards/9628-postgresql-database/)
* [Open Search Dashboard](https://grafana.com/grafana/dashboards/15178-opensearch-prometheus/)  

The following process explains the process to import existing dashboards in Grafana.

* Refer to the [Grafana dashboard repository](https://grafana.com/grafana/dashboards/) for available dashboards.

* Review the available dashboard that meet your specific requirements.

* Refer to the identified dashboard for its specific exporter configuration requirements.

* Follow the steps to import the dashboard as described in the [article](https://grafana.com/docs/grafana/latest/dashboards/manage-dashboards/#export-and-import-dashboards). 
