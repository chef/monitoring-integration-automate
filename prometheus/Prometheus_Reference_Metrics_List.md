# Reference Metrics List

The following section lists/documents the metrics collected by various exporters used for Chef Managed Automate HA implementation. The similar metrics may be collected from aws hosted deployments.

**Disclaimer**
The following metrics are recommended to monitor Chef Automate HA implementation.These metrics provides guidance to use and built monitoring rules and dashboard based on these metrics. However, the actual usage and adoption of metrics depends on each organizational infrastructure monitoring policies.

## System Metrics
* Refer to the following exporters for the metric details.
    - [Node-Exporter](https://github.com/prometheus/node_exporter)
   
 
* The following metrics are configured to generate alerts.  

    | **Component**           | **Metrics Expr**                                    |  
    |-------------------------|------------------------------------------------|  
    |   CPU Usage             | 100 - (avg(irate(node_cpu_seconds_total{mode="idle"}[5m])) by(instance,job) * 100) > 95 |  
    |   CPU Steal | (avg(irate(node_cpu_seconds_total{mode="steal"}[5m]) * 100) by(instance,job))> 20 |
    |   System Memory Usage | 100 - (node_memory_MemAvailable_bytes/node_memory_MemTotal_bytes*100) > 95 |
    |   Disk Utilization | 100 - (node_filesystem_avail_bytes{mountpoint="/"}/node_filesystem_size_bytes{mountpoint="/"}*100) > 85 |
    |   Disk Utilization | 100 - (node_filesystem_avail_bytes{mountpoint="/"}/node_filesystem_size_bytes{mountpoint="/"}*100) > 90 |
    |   Disk Utilization | 100 - (node_filesystem_avail_bytes{mountpoint="/hab"}/node_filesystem_size_bytes{mountpoint="/hab"}*100) > 85 |
    |   Disk Utilization | 100 - (node_filesystem_avail_bytes{mountpoint="/hab"}/node_filesystem_size_bytes{mountpoint="/hab"}*100) > 90 |
    |   Disk Utilization | 100 - (node_filesystem_avail_bytes{mountpoint="/tmp"}/node_filesystem_size_bytes{mountpoint="/tmp"}*100) > 85 |
    |   Disk Utilization | 100 - (node_filesystem_avail_bytes{mountpoint="/tmp"}/node_filesystem_size_bytes{mountpoint="/tmp"}*100) > 90 |
    |  Host Monitoring | up == 0 |


## Chef Automate Health Metrics
* Refer to the following exporters for the metric details.  
        - [Black-Box Exporter](https://github.com/prometheus/blackbox_exporter)  
        - [Nginx-Exporter](https://github.com/nginxinc/nginx-prometheus-exporter)

* The following metrics are configured to generate alerts.


    | **Component**           | **Metrics Expr**                               |
    |-------------------------|------------------------------------------------|
    | Hab Service Status    | probe_http_status_code{job=~"chef-server-services.*|automate-services.*"} != 200 |
    | Hab Service Status | probe_http_status_code{job=~"chef-server-services.*|automate-services.*"} != 200 |
    | Automate LB 5XX Alert | probe_http_status_code{job=~"chef-server-url|chef-automate-url"} >= 500 |
    | Chef-Server LB 5XX Alert | probe_http_status_code{job=~"chef-server-url|chef-automate-url"} >= 500 |

## OpenSearch Metrics
* Refer to the following OpenSearch plugin for the metric details.  
    - [OpenSearch Plug-in](https://github.com/aiven/prometheus-exporter-plugin-for-opensearch)

* The following metrics are configured to generate alerts.


    | **Component**           | **Metrics Expr**                               |
    |-------------------------|------------------------------------------------|
    | ES Cluster Health Check | opensearch_cluster_nodes_number < 2 |
    | ES Heap Usage Factor | opensearch_jvm_mem_heap_used_percent > 95 |
    | ES Performance Alert | opensearch_index_search_fetch_time_seconds > 30 | 
    | ES Performance Alert | opensearch_index_search_fetch_time_seconds > 60 |
    | ES Indexing latency Alert | opensearch_index_indexing_index_time_seconds > 500 |
    | Elasticsearch Search latency Alert | opensearch_index_search_query_time_seconds > 60 |


## Postgres Metrics
*  Refer to the following OpenSearch plugin for the metric details.  
        - [Postgres-Exporter](https://github.com/prometheus-community/postgres_exporter)

* The following metrics are configured to generate alerts.

    | **Component**           | **Metrics Expr**                               |
    |-------------------------|------------------------------------------------|
    | PG Can Connect | pg_up != 1 |
    | Connection Exhaustion | (sum(pg_stat_database_numbackends{server="10.100.12.36:5432"}) by(instance,job))/(avg(pg_settings_max_connections{server="10.100.12.36:5432"}) by(instance,job)) * 100 > 90 |
    | Connection Exhaustion | (sum(pg_stat_database_numbackends{server="10.100.12.36:5432"}) by(instance))/(avg(pg_settings_max_connections{server="10.100.12.36:5432"}) by(instance)) * 100 > 95 |
    | Managed PostgreSQL Write Latency | irate(node_disk_write_time_seconds_total{instance=~".\*pg.\*"}[5m]) / irate(node_disk_writes_completed_total{instance=~".\*pg.\*"}[5m]) > 300 |
    | Managed PostgreSQL Read Latency | irate(node_disk_read_time_seconds_total{instance=~".\*pg.\*"}[5m]) / irate(node_disk_reads_completed_total{instance=~".\*pg.\*"}[5m]) > 300 |

