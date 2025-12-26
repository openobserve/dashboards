# Scylla Metrics Dashboard for OpenObserve

Production monitoring dashboard for ScyllaDB clusters with **7 tabs** and **94 panels**.

## Dashboard Overview

| Tab | Panels | Focus Area | Key Metrics |
|-----|--------|------------|-------------|
| **Cluster Overview** | 15 | Node health & cluster status | Operation mode, live/unreachable nodes, connections, bootstrap status, Scylla version |
| **Operations & Latency** | 15 | Request throughput & performance | Read/write rates, failures, timeouts, latency, CQL operations, reactor stalls |
| **Keyspace & Tables** | 18 | Keyspace & table-level metrics | Disk usage, SSTables, read/write latency (P99), memtable operations, view updates |
| **Cache & Memory** | 14 | Memory efficiency | Cache hit ratios, partition/row cache, LSA memory, dirty bytes, prepared statements |
| **Storage & Compaction** | 10 | Storage health | Active/pending compactions, SSTable counts, compaction backlog |
| **I/O & System** | 10 | System performance | CPU utilization, disk I/O, reactor tasks, AIO operations, I/O queue |
| **Cluster Internals** | 12 | Cluster coordination | Hints, gossip messages, streaming, repair operations |

## Features

- **Multi-node monitoring** with customizable node/shard filters
- **Keyspace and table-level** drill-down capabilities
- **Advanced diagnostics** including tombstone tracking and view sync monitoring
- **Per-datacenter RPC connection** monitoring
- **Real-time alerts** for cluster warnings and abnormal status

## Import

Import `Scylla Metrics Dashboard.dashboard.json` into OpenObserve.
