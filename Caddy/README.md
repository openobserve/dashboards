# Caddy Web Server Dashboard for OpenObserve

Production monitoring dashboard for Caddy reverse proxy with **4 tabs** and **33 panels**.

## Dashboard Overview

| Tab | Panels | Focus Area | Key Metrics |
|-----|--------|------------|-------------|
| **Caddy Overview** | 8 | Server & upstream health | Config reload status, healthy/unhealthy upstreams, health percentage, uptime, admin API requests |
| **Resource Utilization** | 9 | System resources | CPU usage, resident/virtual/heap memory, file descriptors (open/max/utilization %) |
| **Go Runtime** | 10 | Go performance | Goroutines, threads, GC pause (P50/P99), GC cycles, heap objects, allocation rate, stack memory |
| **System Info** | 6 | Build & version info | Caddy/Go version, config reload timestamp, memory frees/mallocs rate, promhttp requests |

## Features

- **Upstream health monitoring** with per-backend status tracking
- **Critical alerts** for unhealthy backends and failed config reloads
- **Resource tracking** for CPU, memory, and file descriptor limits
- **Go runtime insights** including GC performance and goroutine leaks
- **Multi-host filtering** for clusters or load-balanced setups
- **Per-upstream filtering** for granular backend monitoring
- **Admin API visibility** for configuration management tracking

## Prerequisites

- Caddy v2.x with Prometheus metrics enabled
- Prometheus scraping Caddy metrics endpoint
- OpenObserve instance configured to receive Prometheus metrics

## Metrics Endpoint

Enable Prometheus metrics in Caddyfile:

```caddyfile
{
    servers {
        metrics
    }
}

:2019 {
    metrics /metrics
}
```

Prometheus will scrape `http://caddy:2019/metrics`

## Import

Import `Caddy Metrics Dashboard.dashboard.json` into OpenObserve.

## Variables

- **Host**: Filter by specific Caddy server hostname
- **Upstream Backend**: Filter by specific reverse proxy backend

Both variables support multi-select with "all" option for aggregate views.

## Critical Alerts

### Must Be Monitored
- **Config Reload Status** - Must be 1 (green). 0 = failed reload
- **Unhealthy Upstreams** - Non-zero = backend DOWN, requires immediate action
- **Upstream Health %** - Target: 100%. Below 100% = degraded service
- **FD Utilization %** - Alert if > 80% (approaching system limit)

### Performance Indicators
- **GC Pause (P99)** - Should be < 10ms. Higher = latency issues
- **Goroutines** - Sudden increases = potential goroutine leak
- **Memory Allocation Rate** - Steady rate is normal, spikes need investigation
- **CPU Usage** - Baseline monitoring for capacity planning

## Key Metrics Explained

### Upstream Health
- **1** = Healthy (backend responding to health checks)
- **0** = Unhealthy (backend unreachable or failing health checks)

### Memory Types
- **Resident (RSS)** - Physical memory used (primary metric)
- **Virtual** - Total address space (includes mapped files)
- **Heap** - Go-managed memory for objects
- **Stack** - Memory for goroutine stacks

### File Descriptors
- Monitor open FDs vs max limit
- Each connection, file, and socket uses one FD
- Hitting the limit causes "too many open files" errors
