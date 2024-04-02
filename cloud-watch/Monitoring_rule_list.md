# Reference monitoring rule list

**Note:** These are reference monitoring rules; please take this as a guide for other platforms and On-Prem solutions where you will be hosting Automate HA. These rules are guidance only and should depend upon organization-level compliance and audit rules.

| **Service** | **AlertName** | **Rule** | **Severity Level** | **Trigger After** | **Alert Type** | **Comments** |
| ----------- | ------------- | -------- | ------------------ | ----------------- | -------------- | ------------ |
OpenSearch    | ES Cluster Health Check |   elasticsearch.cluster_status < 2 |  L1 |    5 Minutes | PagerDuty | If Cluster Goes to Yellow or Red Status PagerDuty Alert will be triggered |
OpenSearch     |    ES Heap Usage Factor |  (jvm.mem.heap_used/jvm.mem.heap_max) > 0.95  | L1    | 5 Minutes     | PagerDuty |
OpenSearch      |   ES Performance Alert |  elasticsearch.search.fetch.time > 30 sec     | L2    | 10 Minutes    | Slack |
OpenSearch      |   ES Performance Alert |  elasticsearch.search.fetch.time > 60 sec     | L1    | 10 Minutes    | PagerDuty |
OpenSearch      |   ES Indexing latency Alert | avg:aws.es.indexing_latency> 500 ms  | L1    | 15 Minutes    | PagerDuty |
OpenSearch      |   Elasticsearch Search latency Alert |    avg:aws.es.search_latency> 60ms |   L1   | 15 Minutes    | PagerDuty |
Postgresql      |   PG Can Connect |    Check Status (Postgres - Can Connect)    | L1   | 1 Minute   | PagerDuty |
Postgresql      |   Connection Exhaustion | postgresql.percent_usage_connections > 90    | L2    | 10 Minutes    | Slack |
Postgresql |    Connection Exhaustion | postgresql.percent_usage_connections > 95    | L1    | 10 Minutes    | PagerDuty |
Postgresql |    Usage Anomaly | Anomaly on (postgresql.active_queries)   | L2    | 5 Minutes     | Slack |
Postgresql |    Usage Anomaly | Anomaly on (postgresql.active_queries)   | L1    | 10 Minutes    | PagerDuty |
Postgresql |    Usage Anomaly | Anomaly on (postgresql.waiting_queries)  | L2    | 5 Minutes     | Slack |
Postgresql |    Usage Anomaly | Anomaly on (postgresql.waiting_queries)  | L1    | 10 Minutes    | PagerDuty |
Postgresql |    Usage Anomaly | Anomaly on (postgresql.waiting_queries)  | L1    | 10 Minutes    | PagerDuty |
Postgresql |    Managed PostgreSQL Disk Usage Alert |   Disk usage> 90%  | L1    | 10 Minutes    | PagerDuty |
Postgresql |    Managed PostgreSQL Freeable Memory |    avg:aws.rds.freeable_memory < 200000000 MB   | L1    | 10 Minutes    | PagerDuty |
Postgresql |    Managed PostgreSQL Write Latency |  avg:aws.rds.write_latency>300 ms     | L2    | 10 Minutes    | Slack |
Postgresql |    Managed PostgreSQL Read Latency  | avg:aws.rds.read_latency>300 ms   | L2    | 10 Minutes    | Slack |
System |    CPU Usage | (system.cpu.system+system.cpu.user+system.cpu.stolen+system.cpu.guest+system.cpu.iowait) > 95    | L1    | 10 Minutes    | PagerDuty |
System |    CPU Steal | system.cpu.steal > 20    | L1   | 10 Minutes     | PagerDuty |  Terminate if Part of AutoScaling / Stop and Restart machine
System |    System Memory Usage |   system.mem.used/system.mem.total > 0.95  | L2    | 10 Minutes    | Slack |
System |    Disk Utilization |  100*system.disk.used/system.disk.total > 85  | L2    | 10 Minutes    | Slack |
System |    Disk Utilization |  100*system.disk.used/system.disk.total > 90  | L1    | 10 Minutes    | PagerDuty |
System |    Disk Latency |  Anomaly on (system.io.await)     | L2    | 5 Minutes     | Slack |
System |    Host Monitoring  | If a host stops reporting     | L1    | 5 Minutes     | PagerDuty |
Automate |  Hab Service Status |    Check Name (HTTP)    | L1    | 5 Minutes     | PagerDuty |
Chef Server |   Hab Service Status |    Check Name (HTTP)    | L1    | 5 Minutes     | PagerDuty |
Automate ALB |  Automate LB 5XX Alert | ( avg:aws.applicationelb.httpcode_elb_5xx{x-application:saas_automate_lb,x-production:true} by {x-customer}.as_count() / avg:aws.applicationelb.request_count{x-application:saas_automate_lb,x-production:true} by {x-customer}.as_count() ) * 100   | L1    | 10 Minutes    | PagerDuty |
Chef Server ALB |   Chef-Server LB 5XX Alert |  ( avg:aws.applicationelb.httpcode_elb_5xx{x-application:saas_chef-server_lb,x-production:true} by {x-customer}.as_count() / avg:aws.applicationelb.request_count{x-application:saas_chef-server_lb,x-production:true} by {x-customer}.as_count() ) * 100     | L1    | 10 Minutes    | PagerDuty |
