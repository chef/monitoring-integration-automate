# Reference metrics that are used by Datadog

**Note:**
These are AWS metrics, please take this as a guidance for other platforms and On-Prem solutions where you will be hosting Automate HA.


| **Component**               | **Metrics**                                                              |
|-------------------------|----------------------------------------------------------------------|
| OpenSearch              | aws.es.opensearch_dashboards_reporting_request_count                 |
| OpenSearch              | aws.es.opensearch_dashboards_reporting_success_count                 |
| OpenSearch              | aws.es.opensearch_dashboards_reporting_failed_request_sys_err_count  |
| OpenSearch              | aws.es.opensearch_dashboards_reporting_failed_request_user_err_count |
| OpenSearch              | aws.es.open_search_requests                                          |
| OpenSearch              | aws.es.open_search_requests.average                                  |
| OpenSearch              | aws.es.open_search_dashboards_heap_used                              |
| OpenSearch              | aws.es.open_search_dashboards_heap_total                             |
| OpenSearch              | aws.es.open_search_dashboards_healthy_node                           |
| OpenSearch              | aws.es.open_search_dashboards_healthy_nodes                          |
| OpenSearch              | aws.es.open_search_dashboards_request_total                          |
| OpenSearch              | aws.es.open_search_dashboards_os_1minute_load                        |
| OpenSearch              | aws.es.open_search_dashboards_heap_utilization                       |
| OpenSearch              | aws.es.open_search_dashboards_concurrent_connections                 |
| OpenSearch              | aws.es.open_search_dashboards_response_times_max_in_millis           |
| Postgres                | aws.rds.dbload                                                       |
| Postgres                | aws.rds.deadlocks                                                    |
| Postgres                | aws.rds.read_iops                                                    |
| Postgres                | aws.rds.dbload_cpu                                                   |
| Postgres                | aws.rds.swap_usage                                                   |
| Postgres                | aws.rds.vcpu.count                                                   |
| Postgres                | aws.rds.write_iops                                                   |
| Postgres                | aws.rds.replica_lag                                                  |
| Postgres                | aws.rds.ebsiobalance                                                 |
| Postgres                | aws.rds.read_latency                                                 |
| Postgres                | aws.rds.burst_balance                                                |
| Postgres                | aws.rds.engine_uptime                                                |
| Postgres                | aws.rds.write_latency                                                |
| Postgres                | aws.rds.checkpoint_lag                                               |
| Postgres                | aws.rds.commit_latency                                               |
| Postgres                | aws.rds.cpuutilization                                               |
| Postgres                | aws.rds.dbload_non_cpu                                               |
| Postgres                | aws.rds.cpucredit_usage                                              |
| Postgres                | aws.rds.ebsbyte_balance                                              |
| Postgres                | aws.rds.freeable_memory                                              |
| Postgres                | aws.rds.read_throughput                                              |
| Postgres                | aws.rds.disk_queue_depth                                             |
| Postgres                | aws.rds.volume_read_iops                                             |
| Postgres                | aws.rds.write_throughput                                             |
| Postgres                | aws.rds.commit_throughput                                            |
| Postgres                | aws.rds.cpucredit_balance                                            |
| Postgres                | aws.rds.volume_bytes_used                                            |
| Postgres                | aws.rds.volume_write_iops                                            |
| Postgres                | aws.rds.aurora_replica_lag                                           |
| Postgres                | aws.rds.free_local_storage                                           |
| Postgres                | aws.rds.free_storage_space                                           |
| Postgres                | aws.rds.network_throughput                                           |
| Postgres                | aws.rds.total_storage_space                                          |
| Postgres                | aws.rds.database_connections                                         |
| Postgres                | aws.rds.snapshot_storage_used                                        |
| Postgres                | aws.rds.buffer_cache_hit_ratio                                       |
| Postgres                | aws.rds.cpusurplus_credit_balance                                    |
| Postgres                | aws.rds.aurora_replica_lag_maximum                                   |
| Postgres                | aws.rds.aurora_replica_lag_minimum                                   |
| Postgres                | aws.rds.cpusurplus_credits_charged                                   |
| Postgres                | aws.rds.network_receive_throughput                                   |
| Postgres                | aws.rds.storage_network_throughput                                   |
| Postgres                | aws.rds.network_transmit_throughput                                  |
| Postgres                | aws.rds.oldest_replication_slot_lag                                  |
| Postgres                | aws.rds.replication_slot_disk_usage                                  |
| Postgres                | aws.rds.total_backup_storage_billed                                  |
| Postgres                | aws.rds.storage_network_transmit_throughput                          |
| Postgres                | aws.rds.backup_retention_period_storage_used                         |
| Status code             | aws.es.4xx.average                                                   |
| Status code             | aws.elb.httpcode_elb_4xx                                             |
| Status code             | aws.elb.httpcode_target_4xx                                          |
| Status code             | aws.elb.httpcode_backend_4xx                                         |
| Status code             | aws.es.4xx                                                           |
| Status code             | aws.s3.4xx_errors                                                    |
| Status code             | aws.applicationelb.httpcode_elb_4xx                                  |
| Status code             | aws.applicationelb.httpcode_target_4xx                               |
| Status code             | aws.es.5xx                                                           |
| Status code             | aws.s3.5xx_errors                                                    |
| Status code             | aws.es.5xx.average                                                   |
| Status code             | aws.elb.httpcode_elb_5xx                                             |
| Status code             | aws.elb.httpcode_target_5xx                                          |
| Status code             | aws.applicationelb.httpcode_elb_5xx                                  |
| Status code             | aws.applicationelb.httpcode_target_5xx                               |
| System Disk             | system.disk.free                                                     |
| System Disk             | system.disk.used                                                     |
| System Disk             | system.disk.total                                                    |
| System Disk             | system.disk.in_use                                                   |
| System Disk             | system.disk.read_time                                                |
| System Disk             | system.disk.write_time                                               |
| System Disk             | system.disk.read_time_pct                                            |
| System Disk             | system.disk.write_time_pct                                           |
| System Disk             | system.linux.disk_space_utilization                                  |
| System Disk             | system.linux.disk_space_utilization.sum                              |
| System Disk             | system.linux.disk_space_utilization.maximum                          |
| System Disk             | system.linux.disk_space_utilization.minimum                          |
| System Disk             | system.linux.disk_space_utilization.samplecount                      |
| System Disk             | systemd.service.task_count                                           |
| System Memory           | system.mem.free                                                      |
| System Memory           | system.mem.slab                                                      |
| System Memory           | system.mem.used                                                      |
| System Memory           | system.mem.total                                                     |
| System Memory           | system.mem.cached                                                    |
| System Memory           | system.mem.shared                                                    |
| System Memory           | system.mem.usable                                                    |
| System Memory           | system.mem.buffered                                                  |
| System Memory           | system.mem.pct_usable                                                |
| System Memory           | system.mem.page_tables                                               |
| System Memory           | system.mem.commit_limit                                              |
| System Memory           | system.mem.committed_as                                              |
| System Memory           | system.mem.slab_reclaimable                                          |
| System Memory           | systemd.service.memory_usage                                         |
| System Memory           | system.linux.memory_utilization                                      |
| System Memory           | system.linux.memory_utilization.sum                                  |
| System Memory           | system.linux.memory_utilization.maximum                              |
| System Memory           | system.linux.memory_utilization.minimum                              |
| System Memory           | system.linux.memory_utilization.samplecount                          |
| System Memory           | system.net.conntrack.tcp_timeout_max_retrans                         |
| Cloud Host machine(AWS) | aws.ec2.host_ok                                                      |
| Cloud Host machine(AWS) | aws.elb.healthy_host_count                                           |
| Cloud Host machine(AWS) | aws.elb.un_healthy_host_count                                        |
| Cloud Host machine(AWS) | aws.elb.healthy_host_count.maximum                                   |
| Cloud Host machine(AWS) | aws.elb.healthy_host_count.minimum                                   |
| Cloud Host machine(AWS) | aws.elb.healthy_host_count_deduped                                   |
| Cloud Host machine(AWS) | aws.es.invalid_host_header_requests                                  |
| Cloud Host machine(AWS) | aws.networkelb.un_healthy_host_count                                 |
| Cloud Host machine(AWS) | aws.elb.un_healthy_host_count.maximum                                |
| Cloud Host machine(AWS) | aws.elb.un_healthy_host_count.minimum                                |
| Cloud Host machine(AWS) | aws.elb.un_healthy_host_count_deduped                                |
| HTTP connection         | network.http.can_connect                                             |
| HTTP connection         | network.http.cant_connect                                            |
| HTTP connection         | network.http.response_time                                           |
| HTTP connection         | aws.applicationelb.httpredirect                                      |
| HTTP connection         | aws.applicationelb.httpcode_elb_3xx                                  |
| HTTP connection         | aws.applicationelb.httpcode_elb_4xx                                  |
| HTTP connection         | aws.applicationelb.httpcode_elb_5xx                                  |
| HTTP connection         | aws.applicationelb.httpcode_elb_5_0_2                                |
| HTTP connection         | aws.applicationelb.httpcode_elb_5_0_3                                |
| HTTP connection         | aws.applicationelb.httpcode_elb_5_0_4                                |
| HTTP connection         | aws.applicationelb.httpfixed_response                                |
| HTTP connection         | aws.applicationelb.httpcode_target_2xx                               |
| HTTP connection         | aws.applicationelb.httpcode_target_3xx                               |
| HTTP connection         | aws.applicationelb.httpcode_target_4xx                               |
| HTTP connection         | aws.applicationelb.httpcode_target_5xx                               |
| System IO               | system.io.await                                                      |
| System IO               | system.cpu.iowait                                                    |
| System IO               | system.core.iowait                                                   |
| System IO               | system.net.ip.out_discards                                           |
| System IO               | system.net.ip.out_requests                                           |
| System CPU              | system.cpu.idle                                                      |
| System CPU              | system.cpu.user                                                      |
| System CPU              | system.cpu.guest                                                     |
| System CPU              | system.cpu.iowait                                                    |
| System CPU              | system.cpu.stolen                                                    |
| System CPU              | system.cpu.system                                                    |
| System CPU              | system.cpu.interrupt                                                 |
| System CPU              | system.cpu.num_cores                                                 |
| System CPU              | system.cpu.context_switches                                          |
| AWS EC2                 | aws.ec2.host_ok                                                      |
| AWS EC2                 | aws.ec2.network_in                                                   |
| AWS EC2                 | aws.ec2.ebsread_ops                                                  |
| AWS EC2                 | aws.ec2.network_out                                                  |
| AWS EC2                 | aws.ec2.ebsiobalance                                                 |
| AWS EC2                 | aws.ec2.ebswrite_ops                                                 |
| AWS EC2                 | aws.ec2.disk_read_ops                                                |
| AWS EC2                 | aws.ec2.ebsread_bytes                                                |
| AWS EC2                 | aws.ec2.cpuutilization                                               |
| AWS EC2                 | aws.ec2.disk_write_ops                                               |
| AWS EC2                 | aws.ec2.ebswrite_bytes                                               |
| AWS EC2                 | aws.ec2.disk_read_bytes                                              |
| AWS EC2                 | aws.ec2.ebsbyte_balance                                              |
| AWS EC2                 | aws.ec2.ebsread_ops.sum                                              |
| AWS EC2                 | aws.ec2.disk_write_bytes                                             |
| AWS EC2                 | aws.ec2.ebswrite_ops.sum                                             |
| AWS EC2                 | aws.ec2.ebsread_bytes.sum                                            |
| AWS EC2                 | aws.ec2.metadata_no_token                                            |
| AWS EC2                 | aws.ec2.ebswrite_bytes.sum                                           |
| AWS EC2                 | aws.ec2.network_in.maximum                                           |
| AWS EC2                 | aws.ec2.network_packets_in                                           |
| AWS EC2                 | aws.ec2.network_out.maximum                                          |
| AWS EC2                 | aws.ec2.network_packets_out                                          |
| AWS EC2                 | aws.ec2.status_check_failed                                          |
| AWS EC2                 | aws.ec2.cpuutilization.maximum                                       |
| AWS EC2                 | aws.ec2.status_check_failed_system                                   |
| AWS EC2                 | aws.ec2.status_check_failed_instance                                 |
| AWS WAF                 | aws.wafv2.allowed_requests                                           |
| AWS WAF                 | aws.wafv2.blocked_requests                                           |
| AWS WAF                 | aws.wafv2.counted_requests                                           |
| AWS NAT GW              | aws.natgateway.bytes_in_from_source                                  |
| AWS NAT GW              | aws.natgateway.packets_in_from_source                                |
| AWS NAT GW              | aws.natgateway.bytes_in_from_source.sum                              |
| AWS NAT GW              | aws.natgateway.bytes_in_from_destination                             |
| AWS NAT GW              | aws.natgateway.packets_in_from_source.sum                            |
| AWS NAT GW              | aws.natgateway.packets_in_from_destination                           |
| AWS NAT GW              | aws.natgateway.bytes_in_from_destination.sum                         |
| AWS NAT GW              | aws.natgateway.packets_in_from_destination.sum                       |
| AWS S3                  | aws.es.3xx                                                           |
|                         | aws.s3.4xx_errors                                                    |
|                         | aws.s3.5xx_errors                                                    |
|                         | aws.s3.all_requests                                                  |
|                         | aws.s3.get_requests                                                  |
|                         | aws.s3.put_requests                                                  |
|                         | aws.s3.head_requests                                                 |
|                         | aws.s3.bytes_uploaded                                                |
|                         | aws.s3.delete_requests                                               |
|                         | aws.s3.bytes_downloaded                                              |
|                         | aws.s3.bucket_size_bytes                                             |
|                         | aws.s3.number_of_objects                                             |
|                         | aws.s3.first_byte_latency                                            |
|                         | aws.s3.total_request_latency                                         |
|                         | aws.s3.first_byte_latency.p50                                        |
|                         | aws.s3.first_byte_latency.p90                                        |
|                         | aws.s3.first_byte_latency.p95                                        |
|                         | aws.s3.first_byte_latency.p99                                        |
|                         | aws.s3.first_byte_latency.p99.99                                     |
|                         | aws.s3.total_request_latency.p50                                     |
|                         | aws.s3.total_request_latency.p90                                     |
|                         | aws.s3.total_request_latency.p95                                     |
|                         | aws.s3.total_request_latency.p99                                     |
|                         | aws.dd_forwarder.s3_cache_expired                                    |
|                         | aws.s3.first_byte_latency.maximum                                    |
|                         | aws.s3.first_byte_latency.minimum                                    |
|                         | aws.s3.total_request_latency.p99.99                                  |
|                         | aws.s3.total_request_latency.maximum                                 |
|                         | aws.s3.total_request_latency.minimum                                 |