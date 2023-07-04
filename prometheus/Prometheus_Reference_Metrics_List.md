# Reference metrics that are used by Datadog

**Note:**
The following alerting metrics are collected from chef managed deployment. The similar metrics may be collected from aws hosted deployments.


| **Component**               | **Metrics**                                                              |
|-------------------------|----------------------------------------------------------------------|
| OpenSearch              | opensearch_cluster_nodes_number                                      |
| OpenSearch              | opensearch_jvm_mem_heap_used_percent                                 |
| OpenSearch              | opensearch_index_search_fetch_time_seconds                           |
| OpenSearch              | opensearch_index_indexing_index_time_seconds                         |
| OpenSearch              | opensearch_index_search_query_time_seconds                           |
| Postgres                |
| HTTP connection         | probe_http_status_code                                               |
| System Disk             | node_filesystem_avail_bytes                                          |
| System Disk             | node_filesystem_size_bytes                                           |
| Disk Latency            |                                                                      |
| System Memory           | node_memory_MemAvailable_bytes                                       |
| System Memory           | node_memory_MemTotal_bytes                                           |
| System CPU              | node_cpu_seconds_total                                               |
| Host Monitoring         |                                                                      |