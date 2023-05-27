# Datadog Dashboards

## What are Datadog dashboards used for:

+ The dashboard allows us to analyze data from across our entire system in a single pane of glass. It enables our team to immediately benefit from dynamic views with no query language or coding required.

+ Widgets are major building blocks of the dashboard
+ There are various categories internal to the widget which can be chosen based on the view, type, and audience of the information.
+ Category of widget:
  - Generic widgets to graph data
  - Summary widgets to display Synthetic Monitoring information
  - Decoration widgets to visually structures
+ For more information on widgets, kindly refer: https://docs.datadoghq.com/dashboards/widgets/ 


## Considerations for creating Datadog Dashboards

**Define your goals:**
You need to have clarity what you want to show and monitor in your dashboard and your audience; for example, L1 and L2 operations team will require a different set of dashboards versus somebody at the management who wants overall summary on the infrastructure footprint

**Choose the right metrics:**
Not all metrics are created equal, some are of more importance than others. Choose the metrics that are important to you, we have listed down all the important metrics that is critical to your Automate Infrastructure. 

**Use the right visualizations:**
There are a variety of visualizations available in Datadog. Choose the visualizations that are best suited for the metrics you are tracking. For example, if you are tracking CPU usage, you might use a line chart. If you are tracking errors, you might use a heatmap.

**Add context:**
Don't just show your metrics. Add context to your dashboard by adding labels, descriptions, and annotations. This will help you and your team understand what the metrics mean and why they are important.

**Ease of use:**
Your dashboard should be easy to use. Make sure the layout is clear and the visualizations are easy to understand. You can also add filters and alerts to make it even easier to use your dashboard.

## Types of dashboards to consider

**Screenboards:** Screenboards are designed for adhoc analysis and troubleshooting. They allow you to create a custom layout of widgets and panels to visualize your data.

**Timeboards:** Timeboards are designed for monitoring and alerting. They show a historical view of your data over time, and can be used to set alerts and notifications. In our case, we are using Timeboards

# Dashboards that we recommend

Its recommended you have the following dashboards for Automate:

+ Infrastructure Health
+ Component Health
+ Opensearch Metrics 
+ Postgresql Metrics 
+ System Metrics 


For example, a System Metrics dashboard will look like this:

![Image alt text](~dchakrab/Documents/Progress\ Official/Datadog/System\ Metrics\ Dashboard.png)


# Steps to create a new dashboard

+ Login to https://app.datadoghq.com/ with your credentials 
+ On the left hand side, click on Dashboards --> New Dashboard. You will be directed to a new Dashboard screen.
+ Here, key in a name for Dashboard Name and select the "New Timeboard" and then click on "New Dashboard".
+ On the dashboard screen, first create template variables with which you want to tag your components. A basic example can be:

![Image alt text](~dchakrab/Documents/Progress\ Official/Datadog/Template_variables.png)

Once you are done appending your changes, click on save. 

+  Add widgets, select "Timeseries" widget (or your desired widget) and then key in the following parameters in the widget:

   - Select your visualization --> Timeseries
   - Graph your data
     - Metrics --> key in your metrics name; from --> key in tag values you want to filter your view with; select from "avg by"/"max by"/"min by"/"sum by" --> select the relevant filter for your selection
     - Selection of environment, tags
     - Logic: Arithmetic to be applied
   - Set display preferences: 
     - Show --> Global Time
     - Duration of time for data to be shown
   - Key in a title for your widget
   - Once done, click on Save

![Image alt text](~dchakrab/Documents/Progress Official/Datadog/Creating_widget.png)



 # Metrics used in each dashboard
 
 ## Infrastructure health:
 **Chef Automate Status and Infra Server Status:**
- network.http.can_connect; with conditions for Success: cutoff_min --> some_value and Failure: clamp_min --> some_value
  
**Postgres DB Service Status:**
-  postgresql.db.count; with conditions for Success: cutoff_min --> 1 and Failure: cutoff_max --> 0.89

**Opensearch DB Service Status:**
- aws.es.cluster_statusgreen; with conditions for Success: cutoff_min --> 1 and Failure: clamp_min --> 0.5

![Image alt text](~dchakrab/Documents/Progress Official/Datadog/Infra_health_dashboard.png)

 ## Opensearch metrics:

- aws.es.cluster_statusgreen
- aws.es.5xx
- aws.es.indexing_latency
- aws.es.search_latency
- aws.es.indexing_rate
- aws.es.shardsactive
- aws.es.shardsactive
- aws.es.nodes
- aws.es.jvmmemory_pressure
- aws.es.cpuutilization
- aws.es.sys_memory_utilization
- aws.es.threadpool_index_queue
- aws.es.search_rate
- aws.es.searchable_documents
- system.cpu.user
  
 ![Image alt text](~dchakrab/Documents/Progress\ Official/Datadog/AWS_Managed_ES.png)


## Postgresql Metrics:


Assumptions: 
+ We have used a mix of Timeseries, Query Value and Events in the Visualization seclection which is self explanatory
+ Some of the widgets have multiple metrics for creating the desired visualization
+ **Pro-tip:** Clone the built-in AWS Postgresql dashboard and customize accordingly 
  
  - postgresql.percent_usage_connections
  - aws.rds.maximum_used_transaction_ids
  - postgresql.index_scans
  - postgresql.seq_scans
  - postgresql.rows_updated
  - postgresql.rows_deleted
  - postgresql.rows_inserted
  - aws.rds.replica_lag
  - postgresql.locks
  - Source:amazon_rds
  - aws.rds.cpuutilization
  - aws.rds.network_receive_throughput
  - aws.rds.network_transmit_throughput
  - aws.rds.free_storage_space
  - aws.rds.freeable_memory
  - aws.rds.swap_usage
  - aws.rds.read_iops
  - aws.rds.write_iops
  - aws.rds.disk_queue_depth
  - aws.rds.read_latency
  - aws.rds.write_latency

![Image alt text](~dchakrab/Documents/Progress\ Official/Datadog/AWS_RDS.png)



## System Metrics

- system.load.1
- system.load.5
- system.load.15
- system.cpu.system
- system.cpu.iowait
- system.cpu.user
- system.cpu.stolen
- system.cpu.guest
- system.net.bytes_rcvd
- system.mem.usable
- system.mem.total
- system.net.bytes_sent
- system.io.w_s
- system.io.rkb_s
- system.io.wkb_s
- system.disk.used
- system.io.await
- system.cpu.iowait
- system.cpu.stolen
  


![Image alt text](~dchakrab/Documents/Progress\ Official/Datadog/AWS_Opensearch.png)