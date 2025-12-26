# MinIO Object Storage Dashboard for OpenObserve

Production monitoring dashboard for MinIO clusters with **10 tabs** and **81 panels**.

## Dashboard Overview

| Tab | Panels | Focus Area | Key Metrics |
|-----|--------|------------|-------------|
| **Cluster Overview** | 13 | Cluster health & topology | Health status, nodes/drives online/offline, buckets, objects, erasure sets, healing |
| **Storage Capacity** | 9 | Storage utilization | Raw/usable capacity (total/free), storage utilization, disk usage per drive, inodes |
| **S3 Requests** | 9 | S3 API traffic | Requests/sec, errors (4xx/5xx), in-flight requests, traffic sent/received, auth rejections |
| **Drive I/O** | 9 | Drive performance | Operation latency (delete, read, rename, walkdir), I/O errors, timeouts, disk throughput |
| **Inter-Node Traffic** | 5 | Cluster communication | Inter-node bytes sent/received, dial errors, average dial time |
| **Replication & ILM** | 10 | Data management | Replication transfer rate, active workers, backlog, ILM expiry/transition, scanner stats |
| **Resources** | 12 | System resources | CPU, memory (resident/heap), goroutines, file descriptors, syscalls, network, GC pause |
| **Security & KMS** | 4 | KMS integration | KMS online status, request success/errors/failures (4xx/5xx) |
| **Locks** | 3 | Distributed locking | Total distributed locks, read locks, write locks |
| **Object Distribution** | 7 | Object size analytics | Objects by size range (<1KB to >512MB), unversioned objects, delete markers |

## Features

- **Multi-server monitoring** with server and drive filters
- **Comprehensive health checks** for nodes, drives, and erasure sets
- **Real-time S3 metrics** for request rates, errors, and traffic
- **Drive performance tracking** with latency and I/O error monitoring
- **Replication monitoring** with backlog and transfer rate visibility
- **Resource utilization** including CPU, memory, FDs, and network
- **Object size distribution** analysis for capacity planning
- **KMS integration status** for encryption key management

## Prerequisites

- MinIO server with metrics endpoint enabled
- Prometheus scraping MinIO metrics
- OpenObserve instance configured to receive Prometheus metrics

## Import

Import `MinIO Metrics Dashboard.dashboard.json` into OpenObserve.

## Variables

- **Server**: Filter by specific MinIO server nodes
- **Drive**: Filter by specific drive paths

Both variables support multi-select with "all" option for cluster-wide views.
