# RDS Monitoring Dashboard via OpenTelemetry

This repository provides a JSON file for a feature-rich RDS monitoring dashboard on OpenObserve. With this dashboard, you can visualize critical RDS metrics ingested via OpenTelemetry, enabling robust monitoring and proactive management of your database performance.

## Dashboard Features
The JSON file includes panels displaying key RDS metrics, such as:

- CPU Utilization: Monitor RDS instance CPU usage to ensure optimal performance.
- Database Connections: Track the number of active connections to your RDS instances.
- Disk Usage: Observe free storage space and ensure capacity planning.
- Read/Write Latency: Measure query performance and identify potential bottlenecks.
- I/O Operations: Analyze database throughput for read and write operations.
- Network Throughput: Monitor incoming and outgoing data flow for network performance.
- Replication Status: Ensure replication health and identify lag issues.
- Error Logs: Visualize database errors for quick resolution.
- Queries Per Second (QPS): Measure database query execution efficiency.
- Memory Usage: Monitor buffer pool and cache memory to optimize database performance.

![cloudwatch](./screenshots/rds_metrics.gif)