# NATS Messaging Dashboard for OpenObserve

Production monitoring dashboard for NATS messaging systems with **6 tabs** and **54 panels**.

## Dashboard Overview

| Tab | Panels | Focus Area | Key Metrics |
|-----|--------|------------|-------------|
| **NATS Overview** | 10 | Server health & basics | Health status, version, connections, subscriptions, slow consumers, CPU, memory |
| **Messages & Throughput** | 8 | Message flow | Messages in/out, bytes in/out, connection throughput, pending bytes, max payload |
| **Clustering & Routes** | 8 | Cluster topology | Routes, remote servers, leaf nodes, cluster/gateway names, slow consumers by type |
| **JetStream** | 10 | Stream persistence | Memory/storage usage, max limits, API calls/errors, accounts, HA assets, meta leader |
| **Subscriptions** | 8 | Subscription routing | Total subscriptions, cache size/hit rate, matches, inserts/removes, fanout stats |
| **Resources** | 10 | System resources | CPU cores, resident/heap memory, goroutines, threads, FDs, GC pause, network I/O |

## Features

- **Real-time health monitoring** with connection utilization tracking
- **Message throughput analytics** for inbound/outbound traffic
- **JetStream metrics** for persistent streaming and storage
- **Cluster topology visibility** including routes, leaf nodes, and gateways
- **Subscription routing performance** with cache hit rates
- **Resource utilization tracking** for CPU, memory, and goroutines
- **Slow consumer detection** by connection type
- **Multi-server filtering** for cluster-wide or per-server views

## Prerequisites

- NATS server with Prometheus exporter enabled
- Prometheus scraping NATS metrics endpoint
- OpenObserve instance configured to receive Prometheus metrics

## Metrics Exporter

Enable NATS Prometheus exporter:

```conf
# nats-server.conf
http_port: 8222

# Prometheus will scrape http://nats-server:8222/varz
```

## Import

Import `NATS Metrics Dashboard.dashboard.json` into OpenObserve.

## Variables

- **Server**: Filter by specific NATS server ID
  - Supports multi-select with "all" option for cluster-wide monitoring

## Key Panels

### Critical Alerts
- **Health Status** - Must be 1 (healthy)
- **Slow Consumers** - Non-zero indicates client backpressure
- **Connection Utilization** - Warning if >80%
- **JetStream API Errors** - Non-zero requires investigation

### Performance Indicators
- **Messages In/Out** - Throughput monitoring
- **Pending Bytes** - Backpressure indicator
- **Subscription Cache Hit Rate** - Routing efficiency
- **GC Pause (P99)** - Latency impact

### Capacity Planning
- **Connections** vs **Max Connections**
- **JetStream Memory/Storage Used** vs **Max**
- **Open File Descriptors** - Watch for OS limits
