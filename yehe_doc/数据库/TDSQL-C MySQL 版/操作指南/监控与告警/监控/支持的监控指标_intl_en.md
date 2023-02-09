TDSQL-C for MySQL provides monitoring metrics at the instance level.

## Monitoring metrics
>?
>- You can view the following monitoring metrics in the console or through TencentCloud API as instructed in [Viewing Monitoring Data](https://www.tencentcloud.com/document/product/1098/50192).
>- `N` in the monitoring metric unit indicates the unit of the current query time granularity. For example, if the granularity is 5 seconds, `N` will be second (sec); if it is 1 minute, `N` will be minute (min).
>- Parallel query monitoring metrics are supported only for TDSQL-C for MySQL 8.0 (kernel version 3.1.8) or later.

<table>
<thead><tr></tr><th colspan = "2" style="text-align:center" width="30%">Category</th><th>Monitoring Metric</th><th>Parameter</th><th>Unit</th><th>Data Aggregation Method</th></tr></thead>
<tbody>
<tr>
<td rowspan="11" colspan=2>Resource monitoring</td>
<td>CPU Utilization</td><td>cpu_use_rate</td><td>%</td><td>MAX</td></tr>
<td>Memory Utilization</td><td>memory_use_rate</td><td>%</td><td>MAX</td></tr>
<td>Used Memory</td><td>memory_use</td><td>MB</td><td>MAX</td></tr>
<td>Storage Utilization</td><td>storage_use_rate</td><td>%</td><td>MAX</td></tr>
<td>Used Storage Space</td><td>storage_use</td><td>GB</td><td>MAX</td></tr>
<td>Used Data Tablespace</td><td>data_use</td><td>GB</td><td>MAX</td></tr>
<td>Used Temp Tablespace</td><td>tmp_use</td><td>GB</td><td>MAX</td></tr>
<td>Used Undo Tablespace</td><td>undo_use</td><td>GB</td><td>MAX</td></tr>
<td>CCU</td><td>CCU</td><td>-</td><td>MAX</td></tr>
<td>Total Traffic Sent to Client per Sec</td><td>bytes_sent</td><td>MB/s</td><td>MAX</td></tr>
<td>Total Traffic Received from Client per Sec</td><td>bytes_received</td><td>MB/s</td><td>MAX</td></tr>
<tr>
<td rowspan="65">Engine Monitoring</td>
<td rowspan="7">Connection</td>
<td>Executed Queries per Sec</td><td>qps</td><td>Counts/sec</td><td>MAX</td></tr>
<td>Executed Transactions per Sec</td><td>tps</td><td>Counts/sec</td><td>MAX</td></tr>
<td>Connection Utilization</td><td>connection_use_rate</td><td>%</td><td>MAX</td></tr>
<td>Max Connections</td><td>max_connections</td><td>-</td><td>MAX</td></tr>
<td>Open Connections</td><td>threads_connected</td><td>-</td><td>MAX</td></tr>
<td>Created Threads</td><td>threads_created</td><td>-</td><td>SUM</td></tr>
<td>Running Threads</td><td>threads_running</td><td>-</td><td>MAX</td></tr>
<td rowspan="15">Access</td>
<td>Slow Queries</td><td>slow_queries</td><td>-</td><td>SUM</td></tr>
<td>Full-Table Scans</td><td>select_scan</td><td>-</td><td>SUM</td></tr>
<td>SELECT Statements</td><td>com_select</td><td>-</td><td>SUM</td></tr>
<td>UPDATE Statements</td><td>com_update</td><td>-</td><td>SUM</td></tr>
<td>DELETE Statements</td><td>com_delete</td><td>-</td><td>SUM</td></tr>
<td>INSERT Statements</td><td>com_insert</td><td>-</td><td>SUM</td></tr>
<td>REPLACE Queries</td><td>com_replace</td><td>-</td><td>SUM</td></tr>
<td>Total Queries</td><td>queries</td><td>-</td><td>SUM</td></tr>
<td>COMMIT Statements</td><td>com_commit</td><td>-</td><td>SUM</td></tr>
<td>Rollbacks</td><td>com_rollback</td><td>-</td><td>SUM</td></tr>
<td>Full-Table Scan Joins</td><td>select_full_join</td><td>-</td><td>SUM</td></tr>
<td>Range Scan Joins</td><td>select_full_range_join</td><td>-</td><td>SUM</td></tr>
<td>Sort Merges</td><td>sort_merge_passes</td><td>-</td><td>SUM</td></tr>
<td>Qcache Hit Rate</td><td>qcache_hit_rate</td><td>%</td><td>MIN</td></tr>
<td>Qcache Utilization</td><td>qcache_use_rate</td><td>%</td><td>MIN</td></tr>
<td rowspan="6">Table</td>
<td>Temp Tables</td><td>created_tmp_tables</td><td>-</td><td>SUM</td></tr>
<td>Table Lock Waits</td><td>table_locks_waited</td><td>-</td><td>SUM</td></tr>
<td>Opened Tables</td><td>opened_tables</td><td>-</td><td>MAX</td></tr>
<td>Table Locks Released Immediately</td><td>table_locks_immediate</td><td>-</td><td>SUM</td></tr>
<td>Open Table Cache Hits</td><td>table_open_cache_hits</td><td>-</td><td>SUM</td></tr>
<td>Open Table Cache Misses</td><td>table_open_cache_misses</td><td>-</td><td>SUM</td></tr>
<td rowspan="22">InnoDB</td>
<td>InnoDB Cache Hit Rate</td><td>innodb_cache_hit_rate</td><td>%</td><td>MIN</td></tr>
<td>InnoDB Cache Utilization</td><td>innodb_cache_use_rate</td><td>%</td><td>MIN</td></tr>
<td>Disk Reads</td><td>innodb_os_file_reads</td><td>-</td><td>MAX</td></tr>
<td>Disk Writes</td><td>innodb_os_file_writes</td><td>-</td><td>MAX</td></tr>
<td>InnoDB_fsyncs</td><td>innodb_os_fsyncs</td><td>-</td><td>MAX</td></tr>
<td>Tables Opened by InnoDB</td><td>innodb_num_open_files</td><td>-</td><td>MAX</td></tr>
<td>Data Read in InnoDB</td><td>innodb_data_read</td><td>Byte</td><td>SUM</td></tr>
<td>Total InnoDB Reads</td><td>innodb_data_reads</td><td>-</td><td>SUM</td></tr>
<td>Total InnoDB Writes</td><td>innodb_data_writes</td><td>-</td><td>SUM</td></tr>
<td>Data Written in InnoDB</td><td>innodb_data_written</td><td>Byte</td><td>SUM</td></tr>
<td>Rows Deleted from InnoDB Tables</td><td>innodb_rows_deleted</td><td>-</td><td>SUM</td></tr>
<td>Rows Inserted to InnoDB Tables</td><td>innodb_rows_inserted</td><td>-</td><td>SUM</td></tr>
<td>Rows Updated in InnoDB Tables</td><td>innodb_rows_updated</td><td>-</td><td>SUM</td></tr>
<td>Rows Read from InnoDB Tables</td><td>innodb_rows_read</td><td>-</td><td>SUM</td></tr>
<td>Avg Time to Get an InnoDB Row Lock</td><td>innodb_row_lock_time_avg</td><td>ms</td><td>MAX</td></tr>
<td>InnoDB Row Lock Waits</td><td>innodb_row_lock_waits</td><td>-</td><td>SUM</td></tr>
<td>InnoDB Dirty Pages</td><td>innodb_buffer_pool_pages_dirty</td><td>-</td><td>MAX</td></tr>
<td>Writes in InnoDB Suspension</td><td>innodb_data_pending_writes</td><td>-</td><td>MAX</td></tr>
<td>Reads in InnoDB Suspension</td><td>innodb_data_pending_reads</td><td>-</td><td>MAX</td></tr>
<td>Waiting Writes for InnoDB Log</td><td>innodb_log_waits</td><td>-</td><td>SUM</td></tr>
<td>Physical Writes for InnoDB Log</td><td>innodb_log_writes</td><td>-</td><td>SUM</td></tr>
<td>Physical Write Requests for InnoDB Log</td><td>innodb_log_write_requests</td><td>-</td><td>SUM</td></tr>
<td rowspan="2">Tmp</td>
<td>Temp Tables</td><td>created_tmp_disk_tables</td><td>-</td><td>SUM</td></tr>
<td>Temp Files</td><td>created_tmp_files</td><td>-</td><td>SUM</td></tr>
<td rowspan="3">Handler</td>
<td>Requests to Read Next Row</td><td>handler_read_rnd_next</td><td>-</td><td>SUM</td></tr>
<td>Rollbacks Performed in Storage Engine</td><td>handler_rollback</td><td>-</td><td>SUM</td></tr>
<td>Internal COMMIT Statements</td><td>handler_commit</td><td>-</td><td>SUM</td></tr>
<td rowspan="5">Buffer</td>
<td>InnoDB Free Pages</td><td>innodb_buffer_pool_pages_free</td><td>-</td><td>MAX</td></tr>
<td>Total InnoDB Pages</td><td>innodb_buffer_pool_pages_total</td><td>-</td><td>MAX</td></tr>
<td>InnoDB Logical Reads</td><td>innodb_buffer_pool_read_requests</td><td>-</td><td>SUM</td></tr>
<td>InnoDB Physical Reads</td><td>innodb_buffer_pool_reads</td><td>-</td><td>SUM</td></tr>
<td>Writes in InnoDB Buffer Pool</td><td>innodb_buffer_pool_write_request</td><td>-</td><td>SUM</td></tr>
<td rowspan="4">Parallel query</td>
<td>Parallelly Queried Threads	</td><td>txsql_parallel_threads_currently_used</td><td>-</td><td>MAX</td></tr>
<td>Failed Parallel Queries</td><td>txsql_parallel_stmt_error</td><td>-</td><td>SUM</td></tr>
<td>Executed Parallel Queries</td><td>txsql_parallel_stmt_executed</td><td>-</td><td>SUM</td></tr>
<td>Parallel-to-Serial Queries</td><td>txsql_parallel_stmt_fallback</td><td>-</td><td>SUM</td></tr>
<td rowspan="1">Others</td>
<td>Total Opened Files</td><td>open_files</td><td>-</td><td>MAX</td></tr>
<td rowspan="3" colspan=2>Deployment monitoring</td>
<td>Replication Status</td><td>replication_status</td><td>`0`: Yes; `1`: No.</td><td>`1` will be taken if present.</td></tr>
<td>Replication Delay</td><td>replication_delay</td><td>ms</td><td>MAX</td></tr>
<td>Source-Replica Replication LSN Difference</td><td>replication_delay_distance</td><td>Bytes</td><td>MAX</td></tr>
</tbody></table>	

## Performance monitoring data analysis
You can set alarm policies to monitor and analyze performance metrics. Below are some examples:
- CPU utilization: If the CPU utilization meets the application or database requirements such as throughput and concurrency and is expected, then it is appropriate.
- Used storage space: If the used storage space is always greater than or equal to 85% of the total disk space, check how the storage space is used and whether the data can be deleted from the instance or archived to another system to free up space.
- Traffic inbound to/outbound from client: Check the fluctuations of these metrics in **Resource Monitoring** to determine whether the problem is caused by the business peak or database, and then optimize the business or adjust the database configuration.
- Max connections: If there is a large number of connected users, the instance performance drops, and the response time increases, consider restricting the maximum number of connections to the database. The optimal number of user connections is subject to the complexity of operations executed by the instance.

When a performance metric exceeds the configured alarm threshold, you may need to modify relevant parameters to optimize the database availability of the workload.
