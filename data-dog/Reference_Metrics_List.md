# Reference metrics that are used by Datadog

**Note:**
These are  metrics, please take this as a guidance for other platforms and On-Prem solutions where you will be hosting Automate HA.


| **Component**        | **Metrics**                                                      |
| -------------------- | ---------------------------------------------------------------- |
| OpenSearch           | es.opensearch_dashboards_reporting_request_count                 |
| OpenSearch           | es.opensearch_dashboards_reporting_success_count                 |
| OpenSearch           | es.opensearch_dashboards_reporting_failed_request_sys_err_count  |
| OpenSearch           | es.opensearch_dashboards_reporting_failed_request_user_err_count |
| OpenSearch           | es.open_search_requests                                          |
| OpenSearch           | es.open_search_requests.average                                  |
| OpenSearch           | es.open_search_dashboards_heap_used                              |
| OpenSearch           | es.open_search_dashboards_heap_total                             |
| OpenSearch           | es.open_search_dashboards_healthy_node                           |
| OpenSearch           | es.open_search_dashboards_healthy_nodes                          |
| OpenSearch           | es.open_search_dashboards_request_total                          |
| OpenSearch           | es.open_search_dashboards_os_1minute_load                        |
| OpenSearch           | es.open_search_dashboards_heap_utilization                       |
| OpenSearch           | es.open_search_dashboards_concurrent_connections                 |
| OpenSearch           | es.open_search_dashboards_response_times_max_in_millis           |
| Postgres             | rds.dbload                                                       |
| Postgres             | rds.deadlocks                                                    |
| Postgres             | rds.read_iops                                                    |
| Postgres             | rds.dbload_cpu                                                   |
| Postgres             | rds.swap_usage                                                   |
| Postgres             | rds.vcpu.count                                                   |
| Postgres             | rds.write_iops                                                   |
| Postgres             | rds.replica_lag                                                  |
| Postgres             | rds.ebsiobalance                                                 |
| Postgres             | rds.read_latency                                                 |
| Postgres             | rds.burst_balance                                                |
| Postgres             | rds.engine_uptime                                                |
| Postgres             | rds.write_latency                                                |
| Postgres             | rds.checkpoint_lag                                               |
| Postgres             | rds.commit_latency                                               |
| Postgres             | rds.cpuutilization                                               |
| Postgres             | rds.dbload_non_cpu                                               |
| Postgres             | rds.cpucredit_usage                                              |
| Postgres             | rds.ebsbyte_balance                                              |
| Postgres             | rds.freeable_memory                                              |
| Postgres             | rds.read_throughput                                              |
| Postgres             | rds.disk_queue_depth                                             |
| Postgres             | rds.volume_read_iops                                             |
| Postgres             | rds.write_throughput                                             |
| Postgres             | rds.commit_throughput                                            |
| Postgres             | rds.cpucredit_balance                                            |
| Postgres             | rds.volume_bytes_used                                            |
| Postgres             | rds.volume_write_iops                                            |
| Postgres             | rds.aurora_replica_lag                                           |
| Postgres             | rds.free_local_storage                                           |
| Postgres             | rds.free_storage_space                                           |
| Postgres             | rds.network_throughput                                           |
| Postgres             | rds.total_storage_space                                          |
| Postgres             | rds.database_connections                                         |
| Postgres             | rds.snapshot_storage_used                                        |
| Postgres             | rds.buffer_cache_hit_ratio                                       |
| Postgres             | rds.cpusurplus_credit_balance                                    |
| Postgres             | rds.aurora_replica_lag_maximum                                   |
| Postgres             | rds.aurora_replica_lag_minimum                                   |
| Postgres             | rds.cpusurplus_credits_charged                                   |
| Postgres             | rds.network_receive_throughput                                   |
| Postgres             | rds.storage_network_throughput                                   |
| Postgres             | rds.network_transmit_throughput                                  |
| Postgres             | rds.oldest_replication_slot_lag                                  |
| Postgres             | rds.replication_slot_disk_usage                                  |
| Postgres             | rds.total_backup_storage_billed                                  |
| Postgres             | rds.storage_network_transmit_throughput                          |
| Postgres             | rds.backup_retention_period_storage_used                         |
| Status code          | es.4xx.average                                                   |
| Status code          | elb.httpcode_elb_4xx                                             |
| Status code          | elb.httpcode_target_4xx                                          |
| Status code          | elb.httpcode_backend_4xx                                         |
| Status code          | es.4xx                                                           |
| Status code          | s3.4xx_errors                                                    |
| Status code          | applicationelb.httpcode_elb_4xx                                  |
| Status code          | applicationelb.httpcode_target_4xx                               |
| Status code          | es.5xx                                                           |
| Status code          | s3.5xx_errors                                                    |
| Status code          | es.5xx.average                                                   |
| Status code          | elb.httpcode_elb_5xx                                             |
| Status code          | elb.httpcode_target_5xx                                          |
| Status code          | applicationelb.httpcode_elb_5xx                                  |
| Status code          | applicationelb.httpcode_target_5xx                               |
| System Disk          | system.disk.free                                                 |
| System Disk          | system.disk.used                                                 |
| System Disk          | system.disk.total                                                |
| System Disk          | system.disk.in_use                                               |
| System Disk          | system.disk.read_time                                            |
| System Disk          | system.disk.write_time                                           |
| System Disk          | system.disk.read_time_pct                                        |
| System Disk          | system.disk.write_time_pct                                       |
| System Disk          | system.linux.disk_space_utilization                              |
| System Disk          | system.linux.disk_space_utilization.sum                          |
| System Disk          | system.linux.disk_space_utilization.maximum                      |
| System Disk          | system.linux.disk_space_utilization.minimum                      |
| System Disk          | system.linux.disk_space_utilization.samplecount                  |
| System Disk          | systemd.service.task_count                                       |
| System Memory        | system.mem.free                                                  |
| System Memory        | system.mem.slab                                                  |
| System Memory        | system.mem.used                                                  |
| System Memory        | system.mem.total                                                 |
| System Memory        | system.mem.cached                                                |
| System Memory        | system.mem.shared                                                |
| System Memory        | system.mem.usable                                                |
| System Memory        | system.mem.buffered                                              |
| System Memory        | system.mem.pct_usable                                            |
| System Memory        | system.mem.page_tables                                           |
| System Memory        | system.mem.commit_limit                                          |
| System Memory        | system.mem.committed_as                                          |
| System Memory        | system.mem.slab_reclaimable                                      |
| System Memory        | systemd.service.memory_usage                                     |
| System Memory        | system.linux.memory_utilization                                  |
| System Memory        | system.linux.memory_utilization.sum                              |
| System Memory        | system.linux.memory_utilization.maximum                          |
| System Memory        | system.linux.memory_utilization.minimum                          |
| System Memory        | system.linux.memory_utilization.samplecount                      |
| System Memory        | system.net.conntrack.tcp_timeout_max_retrans                     |
| Cloud Host machine() | ec2.host_ok                                                      |
| Cloud Host machine() | elb.healthy_host_count                                           |
| Cloud Host machine() | elb.un_healthy_host_count                                        |
| Cloud Host machine() | elb.healthy_host_count.maximum                                   |
| Cloud Host machine() | elb.healthy_host_count.minimum                                   |
| Cloud Host machine() | elb.healthy_host_count_deduped                                   |
| Cloud Host machine() | es.invalid_host_header_requests                                  |
| Cloud Host machine() | networkelb.un_healthy_host_count                                 |
| Cloud Host machine() | elb.un_healthy_host_count.maximum                                |
| Cloud Host machine() | elb.un_healthy_host_count.minimum                                |
| Cloud Host machine() | elb.un_healthy_host_count_deduped                                |
| HTTP connection      | network.http.can_connect                                         |
| HTTP connection      | network.http.cant_connect                                        |
| HTTP connection      | network.http.response_time                                       |
| HTTP connection      | applicationelb.httpredirect                                      |
| HTTP connection      | applicationelb.httpcode_elb_3xx                                  |
| HTTP connection      | applicationelb.httpcode_elb_4xx                                  |
| HTTP connection      | applicationelb.httpcode_elb_5xx                                  |
| HTTP connection      | applicationelb.httpcode_elb_5_0_2                                |
| HTTP connection      | applicationelb.httpcode_elb_5_0_3                                |
| HTTP connection      | applicationelb.httpcode_elb_5_0_4                                |
| HTTP connection      | applicationelb.httpfixed_response                                |
| HTTP connection      | applicationelb.httpcode_target_2xx                               |
| HTTP connection      | applicationelb.httpcode_target_3xx                               |
| HTTP connection      | applicationelb.httpcode_target_4xx                               |
| HTTP connection      | applicationelb.httpcode_target_5xx                               |
| System IO            | system.io.await                                                  |
| System IO            | system.cpu.iowait                                                |
| System IO            | system.core.iowait                                               |
| System IO            | system.net.ip.out_discards                                       |
| System IO            | system.net.ip.out_requests                                       |
| System CPU           | system.cpu.idle                                                  |
| System CPU           | system.cpu.user                                                  |
| System CPU           | system.cpu.guest                                                 |
| System CPU           | system.cpu.iowait                                                |
| System CPU           | system.cpu.stolen                                                |
| System CPU           | system.cpu.system                                                |
| System CPU           | system.cpu.interrupt                                             |
| System CPU           | system.cpu.num_cores                                             |
| System CPU           | system.cpu.context_switches                                      |
| EC2                  | ec2.host_ok                                                      |
| EC2                  | ec2.network_in                                                   |
| EC2                  | ec2.ebsread_ops                                                  |
| EC2                  | ec2.network_out                                                  |
| EC2                  | ec2.ebsiobalance                                                 |
| EC2                  | ec2.ebswrite_ops                                                 |
| EC2                  | ec2.disk_read_ops                                                |
| EC2                  | ec2.ebsread_bytes                                                |
| EC2                  | ec2.cpuutilization                                               |
| EC2                  | ec2.disk_write_ops                                               |
| EC2                  | ec2.ebswrite_bytes                                               |
| EC2                  | ec2.disk_read_bytes                                              |
| EC2                  | ec2.ebsbyte_balance                                              |
| EC2                  | ec2.ebsread_ops.sum                                              |
| EC2                  | ec2.disk_write_bytes                                             |
| EC2                  | ec2.ebswrite_ops.sum                                             |
| EC2                  | ec2.ebsread_bytes.sum                                            |
| EC2                  | ec2.metadata_no_token                                            |
| EC2                  | ec2.ebswrite_bytes.sum                                           |
| EC2                  | ec2.network_in.maximum                                           |
| EC2                  | ec2.network_packets_in                                           |
| EC2                  | ec2.network_out.maximum                                          |
| EC2                  | ec2.network_packets_out                                          |
| EC2                  | ec2.status_check_failed                                          |
| EC2                  | ec2.cpuutilization.maximum                                       |
| EC2                  | ec2.status_check_failed_system                                   |
| EC2                  | ec2.status_check_failed_instance                                 |
| WAF                  | wafv2.allowed_requests                                           |
| WAF                  | wafv2.blocked_requests                                           |
| WAF                  | wafv2.counted_requests                                           |
| NAT GW               | natgateway.bytes_in_from_source                                  |
| NAT GW               | natgateway.packets_in_from_source                                |
| NAT GW               | natgateway.bytes_in_from_source.sum                              |
| NAT GW               | natgateway.bytes_in_from_destination                             |
| NAT GW               | natgateway.packets_in_from_source.sum                            |
| NAT GW               | natgateway.packets_in_from_destination                           |
| NAT GW               | natgateway.bytes_in_from_destination.sum                         |
| NAT GW               | natgateway.packets_in_from_destination.sum                       |
| Nginx                | nginx.net.writing                                                |
| Nginx                | nginx.net.waiting                                                |
| Nginx                | nginx.net.reading                                                |
| Nginx                | nginx.net.connections                                            |
| Nginx                | nginx.net.request_per_s                                          |
| Nginx                | nginx.net.conn_opened_per_s                                      |
| Nginx                | nginx.net.conn_dropped_per_s                                     |
| Nginx                | nginx.cache.bypass.bytes                                         |
| Nginx                | nginx.cache.bypass.bytes_count                                   |
| Nginx                | nginx.cache.bypass.bytes_written                                 |
| Nginx                | nginx.cache.bypass.bytes_written_count                           |
| Nginx                | nginx.cache.bypass.responses                                     |
| Nginx                | nginx.cache.bypass.responses_count                               |
| Nginx                | nginx.cache.bypass.responses_written                             |
| Nginx                | nginx.cache.bypass.responses_written_count                       |
| Nginx                | nginx.cache.cold                                                 |
| Nginx                | nginx.cache.expired.bytes                                        |
| Nginx                | nginx.cache.expired.bytes_count                                  |
| Nginx                | nginx.cache.expired.bytes_written                                |
| Nginx                | nginx.cache.expired.bytes_written_count                          |
| Nginx                | nginx.cache.expired.responses                                    |
| Nginx                | nginx.cache.expired.responses_count                              |
| Nginx                | nginx.cache.expired.responses_written                            |
| Nginx                | nginx.cache.expired.responses_written_count                      |
| Nginx                | nginx.cache.hit.bytes                                            |
| Nginx                | nginx.cache.hit.bytes_count                                      |
| Nginx                | nginx.cache.hit.responses                                        |
| Nginx                | nginx.cache.hit.responses_count                                  |
| Nginx                | nginx.cache.max_size                                             |
| Nginx                | nginx.cache.miss.bytes                                           |
| Nginx                | nginx.cache.miss.bytes_count                                     |
| Nginx                | nginx.cache.miss.bytes_written                                   |
| Nginx                | nginx.cache.miss.bytes_written_count                             |
| Nginx                | nginx.cache.miss.responses                                       |
| Nginx                | nginx.cache.miss.responses_count                                 |
| Nginx                | nginx.cache.miss.responses_written                               |
| Nginx                | nginx.cache.miss.responses_written_count                         |
| Nginx                | nginx.cache.revalidated.bytes                                    |
| Nginx                | nginx.cache.revalidated.bytes_count                              |
| Nginx                | nginx.cache.revalidated.responses                                |
| Nginx                | nginx.cache.revalidated.responses_count                          |
| Nginx                | nginx.cache.size                                                 |
| Nginx                | nginx.cache.stale.bytes                                          |
| Nginx                | nginx.cache.stale.bytes_count                                    |
| Nginx                | nginx.cache.stale.responses                                      |
| Nginx                | nginx.cache.stale.responses_count                                |
| Nginx                | nginx.cache.updating.bytes                                       |
| Nginx                | nginx.cache.updating.bytes_count                                 |
| Nginx                | nginx.cache.updating.responses                                   |
| Nginx                | nginx.cache.updating.responses_count                             |
| Nginx                | nginx.connections.accepted                                       |
| Nginx                | nginx.connections.accepted_count                                 |
| Nginx                | nginx.connections.active                                         |
| Nginx                | nginx.connections.dropped                                        |
| Nginx                | nginx.connections.dropped_count                                  |
| Nginx                | nginx.connections.idle                                           |
| Nginx                | nginx.generation                                                 |
| Nginx                | nginx.generation_count                                           |
| Nginx                | nginx.limit_conn.passed                                          |
| Nginx                | nginx.limit_conn.rejected                                        |
| Nginx                | nginx.limit_conn.rejected_dry_run                                |
| Nginx                | nginx.limit_req.delayed_dry_run                                  |
| Nginx                | nginx.limit_req.delayed                                          |
| Nginx                | nginx.limit_req.passed                                           |
| Nginx                | nginx.limit_req.rejected_dry_run                                 |
| Nginx                | nginx.limit_req.rejected                                         |
| Nginx                | nginx.location_zone.discarded                                    |
| Nginx                | nginx.location_zone.received                                     |
| Nginx                | nginx.location_zone.requests                                     |
| Nginx                | nginx.location_zone.responses.1xx                                |
| Nginx                | nginx.location_zone.responses.2xx                                |
| Nginx                | nginx.location_zone.responses.3xx                                |
| Nginx                | nginx.location_zone.responses.4xx                                |
| Nginx                | nginx.location_zone.responses.5xx                                |
| Nginx                | nginx.location_zone.responses.code                               |
| Nginx                | nginx.location_zone.responses.total                              |
| Nginx                | nginx.location_zone.sent                                         |
| Nginx                | nginx.load_timestamp                                             |
| Nginx                | nginx.pid                                                        |
| Nginx                | nginx.ppid                                                       |
| Nginx                | nginx.version                                                    |