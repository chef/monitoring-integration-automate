# Reference metrics that are collected by Prometheus

**Note:**
The following alerting metrics are collected from chef managed deployment. The similar metrics may be collected from aws hosted deployments.


| **Component**               | **Metrics**                                                              |
|-------------------------|--------------------------------------------------------------------------------|
| OpenSearch  | opensearch_circuitbreaker_estimated_bytes           |
| OpenSearch  | opensearch_circuitbreaker_limit_bytes               |
| OpenSearch  | opensearch_circuitbreaker_overhead_ratio            |
| OpenSearch  | opensearch_circuitbreaker_tripped_count             |
| OpenSearch  | opensearch_cluster_datanodes_number                 |
| OpenSearch  | opensearch_cluster_inflight_fetch_number            |
| OpenSearch  | opensearch_cluster_is_timedout_bool                 |
| OpenSearch  | opensearch_cluster_nodes_number                     |
| OpenSearch  | opensearch_cluster_pending_tasks_number            |
| OpenSearch  | opensearch_cluster_routing_allocation_disk_threshold_enabled            |
| OpenSearch  | opensearch_cluster_routing_allocation_disk_watermark_flood_stage_pct            |
| OpenSearch  | opensearch_cluster_routing_allocation_disk_watermark_high_pct            |
| OpenSearch  | opensearch_cluster_routing_allocation_disk_watermark_low_pct            |
| OpenSearch  | opensearch_cluster_shards_active_percent            |
| OpenSearch  | opensearch_cluster_shards_number            |
| OpenSearch  | opensearch_cluster_status            |
| OpenSearch  | opensearch_cluster_task_max_waiting_time_seconds            |
| OpenSearch  | opensearch_fs_io_total_operations            |
| OpenSearch  | opensearch_fs_io_total_read_bytes            |
| OpenSearch  | opensearch_fs_io_total_read_operations
opensearch_fs_io_total_write_bytes            |
| OpenSearch  | opensearch_fs_io_total_write_operations            |
| OpenSearch  | opensearch_fs_path_available_bytes            |
| OpenSearch  | opensearch_fs_path_free_bytes
opensearch_fs_path_total_bytes            |
| OpenSearch  | opensearch_fs_total_available_bytes            |
| OpenSearch  | opensearch_fs_total_free_bytes            |
| OpenSearch  | opensearch_fs_total_total_bytes            |
| OpenSearch  | opensearch_http_open_server_number            |
| OpenSearch  | opensearch_http_open_total_count            |
| OpenSearch  | opensearch_index_completion_size_bytes            |
| OpenSearch  | opensearch_index_doc_deleted_number            |
| OpenSearch  | opensearch_index_doc_number            |
| OpenSearch  | opensearch_index_fielddata_evictions_count            |
| OpenSearch  | opensearch_index_fielddata_memory_size_bytes            |
| OpenSearch  | opensearch_index_flush_total_count            |
| OpenSearch  | opensearch_index_flush_total_time_seconds            |
| OpenSearch  | opensearch_index_get_count            |
| OpenSearch  | opensearch_index_get_current_number            |
| OpenSearch  | opensearch_index_get_exists_count            |
| OpenSearch  | opensearch_index_get_exists_time_seconds            |
| OpenSearch  | opensearch_index_get_missing_count            |
| OpenSearch  | opensearch_index_get_missing_time_seconds            |
| OpenSearch  | opensearch_index_get_time_seconds            |
| OpenSearch  | opensearch_index_indexing_delete_count            |
| OpenSearch  | opensearch_index_indexing_delete_current_number            |
| OpenSearch  | opensearch_index_indexing_delete_time_seconds            |
| OpenSearch  | opensearch_index_indexing_index_count            |
| OpenSearch  | opensearch_index_indexing_index_current_number            |
| OpenSearch  | opensearch_index_indexing_index_failed_count            |
| OpenSearch  | opensearch_index_indexing_index_time_seconds            |
| OpenSearch  | opensearch_index_indexing_is_throttled_bool            |
| OpenSearch  | opensearch_index_indexing_noop_update_count            |
| OpenSearch  | opensearch_index_indexing_throttle_time_seconds            |
| OpenSearch  | opensearch_index_merges_current_docs_number            |
| OpenSearch  | opensearch_index_merges_current_number            |
| OpenSearch  | opensearch_index_merges_current_size_bytes            |
| OpenSearch  | opensearch_index_merges_total_auto_throttle_bytes            |
| OpenSearch  | opensearch_index_merges_total_docs_count            |
| OpenSearch  | opensearch_index_merges_total_number            |
| OpenSearch  | opensearch_index_merges_total_size_bytes            |
| OpenSearch  | opensearch_index_merges_total_stopped_time_seconds            |
| OpenSearch  | opensearch_index_merges_total_throttled_time_seconds            |
| OpenSearch  | opensearch_index_merges_total_time_seconds            |
| OpenSearch  | opensearch_index_querycache_cache_count            |
| OpenSearch  | opensearch_index_querycache_cache_size_bytes            |
| OpenSearch  | opensearch_index_querycache_evictions_count            |
| OpenSearch  | opensearch_index_querycache_hit_count            |
| OpenSearch  | opensearch_index_querycache_memory_size_bytes            |
| OpenSearch  | opensearch_index_querycache_miss_number            |
| OpenSearch  | opensearch_index_querycache_total_number            |
| OpenSearch  | opensearch_index_recovery_current_number            |
| OpenSearch  | opensearch_index_recovery_throttle_time_seconds            |
| OpenSearch  | opensearch_index_refresh_listeners_number            |
| OpenSearch  | opensearch_index_refresh_total_count            |
| OpenSearch  | opensearch_index_refresh_total_time_seconds            |
| OpenSearch  | opensearch_index_replicas_number            |
| OpenSearch  | opensearch_index_requestcache_evictions_count            |
| OpenSearch  | opensearch_index_requestcache_hit_count            |
| OpenSearch  | opensearch_index_requestcache_memory_size_bytes            |
| OpenSearch  | opensearch_index_requestcache_miss_count            |
| OpenSearch  | opensearch_index_search_fetch_count            |
| OpenSearch  | opensearch_index_search_fetch_current_number            |
| OpenSearch  | opensearch_index_search_fetch_time_seconds            |
| OpenSearch  | opensearch_index_search_open_contexts_number            |
| OpenSearch  | opensearch_index_search_query_count            |
| OpenSearch  | opensearch_index_search_query_current_number            |
| OpenSearch  | opensearch_index_search_query_time_seconds            |
| OpenSearch  | opensearch_index_search_scroll_count
            |
| OpenSearch  | opensearch_index_search_scroll_current_number            |
| OpenSearch  | opensearch_index_search_scroll_time_seconds
| OpenSearch  | opensearch_index_segments_memory_bytes
| OpenSearch  | opensearch_index_segments_number            |
| OpenSearch  | opensearch_index_shards_number            |
| OpenSearch  | opensearch_index_status
| OpenSearch  | opensearch_index_store_size_bytes
| OpenSearch  | opensearch_index_suggest_count
| OpenSearch  | opensearch_index_suggest_current_number            |
| OpenSearch  | opensearch_index_suggest_time_seconds
| OpenSearch  | opensearch_index_translog_operations_number            |
| OpenSearch  | opensearch_index_translog_size_bytes
| OpenSearch  | opensearch_index_translog_uncommitted_operations_number            |
| OpenSearch  | opensearch_index_translog_uncommitted_size_bytes
| OpenSearch  | opensearch_index_warmer_count
| OpenSearch  | opensearch_index_warmer_current_number            |
| OpenSearch  | opensearch_index_warmer_time_seconds
| OpenSearch  | opensearch_indices_completion_size_bytes
| OpenSearch  | opensearch_indices_doc_deleted_number            |
| OpenSearch  | opensearch_indices_doc_number            |
| OpenSearch  | opensearch_indices_fielddata_evictions_count
| OpenSearch  | opensearch_indices_fielddata_memory_size_bytes
| OpenSearch  | opensearch_indices_flush_total_count
| OpenSearch  | opensearch_indices_flush_total_time_seconds
| OpenSearch  | opensearch_indices_get_count
| OpenSearch  | opensearch_indices_get_current_number            |
| OpenSearch  | opensearch_indices_get_exists_count
| OpenSearch  | opensearch_indices_get_exists_time_seconds
| OpenSearch  | opensearch_indices_get_missing_count
| OpenSearch  | opensearch_indices_get_missing_time_seconds
| OpenSearch  | opensearch_indices_get_time_seconds
| OpenSearch  | opensearch_indices_indexing_delete_count
| OpenSearch  | opensearch_indices_indexing_delete_current_number            |
| OpenSearch  | opensearch_indices_indexing_delete_time_seconds
| OpenSearch  | opensearch_indices_indexing_index_count
| OpenSearch  | opensearch_indices_indexing_index_current_number            |
| OpenSearch  | opensearch_indices_indexing_index_failed_count
| OpenSearch  | opensearch_indices_indexing_index_time_seconds
| OpenSearch  | opensearch_indices_indexing_is_throttled_bool
| OpenSearch  | opensearch_indices_indexing_noop_update_count
| OpenSearch  | opensearch_indices_indexing_throttle_time_seconds
| OpenSearch  | opensearch_indices_merges_current_docs_number            |
| OpenSearch  | opensearch_indices_merges_current_number            |
| OpenSearch  | opensearch_indices_merges_current_size_bytes
| OpenSearch  | opensearch_indices_merges_total_auto_throttle_bytes
| OpenSearch  | opensearch_indices_merges_total_docs_count
| OpenSearch  | opensearch_indices_merges_total_number            |
| OpenSearch  | opensearch_indices_merges_total_size_bytes
| OpenSearch  | opensearch_indices_merges_total_stopped_time_seconds
| OpenSearch  | opensearch_indices_merges_total_throttled_time_seconds
| OpenSearch  | opensearch_indices_merges_total_time_seconds
| OpenSearch  | opensearch_indices_querycache_cache_count
| OpenSearch  | opensearch_indices_querycache_cache_size_bytes
| OpenSearch  | opensearch_indices_querycache_evictions_count
| OpenSearch  | opensearch_indices_querycache_hit_count
| OpenSearch  | opensearch_indices_querycache_memory_size_bytes
| OpenSearch  | opensearch_indices_querycache_miss_number            |
| OpenSearch  | opensearch_indices_querycache_total_number            |
| OpenSearch  | opensearch_indices_recovery_current_number            |
| OpenSearch  | opensearch_indices_recovery_throttle_time_seconds
| OpenSearch  | opensearch_indices_refresh_listeners_number            |
| OpenSearch  | opensearch_indices_refresh_total_count
| OpenSearch  | opensearch_indices_refresh_total_time_seconds
| OpenSearch  | opensearch_indices_requestcache_evictions_count
| OpenSearch  | opensearch_indices_requestcache_hit_count
| OpenSearch  | opensearch_indices_requestcache_memory_size_bytes
| OpenSearch  | opensearch_indices_requestcache_miss_count
| OpenSearch  | opensearch_indices_search_fetch_count
| OpenSearch  | opensearch_indices_search_fetch_current_number            |
| OpenSearch  | opensearch_indices_search_fetch_time_seconds
| OpenSearch  | opensearch_indices_search_open_contexts_number            |
| OpenSearch  | opensearch_indices_search_query_count
| OpenSearch  | opensearch_indices_search_query_current_number            |
| OpenSearch  | opensearch_indices_search_query_time_seconds
| OpenSearch  | opensearch_indices_search_scroll_count
| OpenSearch  | opensearch_indices_search_scroll_current_number            |
| OpenSearch  | opensearch_indices_search_scroll_time_seconds
| OpenSearch  | opensearch_indices_segments_memory_bytes
| OpenSearch  | opensearch_indices_segments_number            |
| OpenSearch  | opensearch_indices_store_size_bytes
| OpenSearch  | opensearch_indices_suggest_count
| OpenSearch  | opensearch_indices_suggest_current_number            |
| OpenSearch  | opensearch_indices_suggest_time_seconds
| OpenSearch  | opensearch_ingest_total_count
| OpenSearch  | opensearch_ingest_total_current
| OpenSearch  | opensearch_ingest_total_failed_count
| OpenSearch  | opensearch_ingest_total_time_seconds
| OpenSearch  | opensearch_jvm_bufferpool_number            |
| OpenSearch  | opensearch_jvm_bufferpool_total_capacity_bytes
| OpenSearch  | opensearch_jvm_bufferpool_used_bytes
| OpenSearch  | opensearch_jvm_classes_loaded_number            |
| OpenSearch  | opensearch_jvm_classes_total_loaded_number            |
| OpenSearch  | opensearch_jvm_classes_unloaded_number            |
| OpenSearch  | opensearch_jvm_gc_collection_count
| OpenSearch  | opensearch_jvm_gc_collection_time_seconds
| OpenSearch  | opensearch_jvm_mem_heap_committed_bytes
| OpenSearch  | opensearch_jvm_mem_heap_max_bytes
| OpenSearch  | opensearch_jvm_mem_heap_used_bytes
| OpenSearch  | opensearch_jvm_mem_heap_used_percent
| OpenSearch  | opensearch_jvm_mem_nonheap_committed_bytes
| OpenSearch  | opensearch_jvm_mem_nonheap_used_bytes
| OpenSearch  | opensearch_jvm_mem_pool_max_bytes
| OpenSearch  | opensearch_jvm_mem_pool_peak_max_bytes
| OpenSearch  | opensearch_jvm_mem_pool_peak_used_bytes
| OpenSearch  | opensearch_jvm_mem_pool_used_bytes
| OpenSearch  | opensearch_jvm_threads_number            |
| OpenSearch  | opensearch_jvm_threads_peak_number            |
| OpenSearch  | opensearch_jvm_uptime_seconds
| OpenSearch  | opensearch_metrics_generate_time_seconds_count
| OpenSearch  | opensearch_metrics_generate_time_seconds_created
| OpenSearch  | opensearch_metrics_generate_time_seconds_sum
| OpenSearch  | opensearch_node_role_bool
| OpenSearch  | opensearch_os_cpu_percent
| OpenSearch  | opensearch_os_load_average_fifteen_minutes            |
| OpenSearch  | opensearch_os_load_average_five_minutes            |
| OpenSearch  | opensearch_os_load_average_one_minute
| OpenSearch  | opensearch_os_mem_free_bytes
| OpenSearch  | opensearch_os_mem_free_percent
| OpenSearch  | opensearch_os_mem_total_bytes
| OpenSearch  | opensearch_os_mem_used_bytes
| OpenSearch  | opensearch_os_mem_used_percent
| OpenSearch  | opensearch_os_swap_free_bytes
| OpenSearch  | opensearch_os_swap_total_bytes
| OpenSearch  | opensearch_os_swap_used_bytes
| OpenSearch  | opensearch_process_cpu_percent
| OpenSearch  | opensearch_process_cpu_time_seconds
| OpenSearch  | opensearch_process_file_descriptors_max_number            |
| OpenSearch  | opensearch_process_file_descriptors_open_number            |
| OpenSearch  | opensearch_process_mem_total_virtual_bytes
| OpenSearch  | opensearch_script_cache_evictions_count            |
| OpenSearch  | opensearch_script_compilations_count            |
| OpenSearch  | opensearch_threadpool_tasks_number
| OpenSearch  | opensearch_threadpool_threads_count            |
| OpenSearch  | opensearch_threadpool_threads_number            |
| OpenSearch  | opensearch_transport_rx_bytes_count            |
| OpenSearch  | opensearch_transport_rx_packets_count            |
| OpenSearch  | opensearch_transport_server_open_number            |
| OpenSearch  | opensearch_transport_tx_bytes_count            |
| OpenSearch  | opensearch_transport_tx_packets_count            |


| Postgres                |
| HTTP connection         | probe_http_status_code                                               |
| System Disk             | node_filesystem_avail_bytes                                          |
| System Disk             | node_filesystem_size_bytes                                           |
| Disk Latency            |                                                                      |
| System Memory           | node_memory_MemAvailable_bytes                                       |
| System Memory           | node_memory_MemTotal_bytes                                           |
| System CPU              | node_cpu_seconds_total                                               |
| Host Monitoring         |                                                                      |