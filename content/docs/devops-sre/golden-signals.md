+++
date = '2025-12-19T22:13:23+02:00'
title = 'Golden Signals'
menus = 'main'
type = 'page'
+++

The Four Golden Signals are a classic observability concept introduced by Google’s SRE team. They describe the four most important metrics to monitor for any user-facing system.

If you track these well, you’ll catch most production issues early.

# The Four Golden Signals

## Latency

How long requests take

* Measures response time
* Track distribution, not just averages
* Separate successful vs failed requests

Examples

* p50 / p95 / p99 response time
* API response duration
* Page load time

⚠️ Averages hide problems — always use percentiles.

##  Traffic

How much demand your system is under

* Requests per second
* Throughput
* Concurrent users
* Examples
* HTTP requests/sec
* Queue depth
* DB queries/sec

Traffic tells you whether problems are load-related.

## Errors

The rate of failing requests

* Explicit failures (5xx, exceptions)
* Implicit failures (wrong responses, timeouts)

Examples

* HTTP 5xx rate
* Error percentage
* Failed background jobs
* Errors without traffic can indicate internal failures.

## Saturation

How “full” your system is

* Measures resource exhaustion
* Indicates proximity to limits
* Examples
* CPU utilization
* Memory usage
* Disk I/O
* Thread pool exhaustion
* Queue backlog

Saturation predicts future failure.

🧠 Mental model

| Signal     | Question it answers    |
| ---------- | ---------------------- |
| Latency    | “Is the system slow?”  |
| Traffic    | “Is it being used?”    |
| Errors     | “Is it failing?”       |
| Saturation | “Is it about to fail?” |

## How they work together

Example scenario:
```
Traffic spikes → Saturation increases → Latency worsens → Errors appear
```
If you only monitor errors, you see the problem last.

🔧 Mapping to common systems

### Web API

* Latency: p95 response time
* Traffic: RPS
* Errors: 5xx %
* Saturation: CPU / memory / connection pool

### Database

* Latency: query time
* Traffic: QPS
* Errors: failed queries
* Saturation: connections, disk IOPS

### Queue

* Latency: message processing delay
* Traffic: enqueue rate
* Errors: failed jobs
* Saturation: queue depth

## Golden Signals vs RED & USE
| Model                                     | Focus               |
| ----------------------------------------- | ------------------- |
| **Golden Signals**                        | User-facing systems |
| **RED** (Rate, Errors, Duration)          | APIs & services     |
| **USE** (Utilization, Saturation, Errors) | Infrastructure      |


They complement each other.

## Summary

The Four Golden Signals are:
* Latency
* Traffic
* Errors
* Saturation

If you monitor these four well, you’ll catch most production incidents early.

## Percentiles

p50 / p95 / p99 are latency percentiles — they tell you how fast (or slow) your system is for most users, not just on average.

They’re fundamental to observability and performance work.

### What does “p” mean?

p = percentile

> “p95 latency = 300 ms” means \
> 95% of requests finished in ≤ 300 ms \
> and 5% took longer.

### Common Percentiles
| Percentile | Meaning         | Who experiences it |
| ---------- | --------------- | ------------------ |
| **p50**    | Median          | Typical user       |
| **p90**    | Slow but common | 1 in 10 users      |
| **p95**    | Very slow       | 1 in 20 users      |
| **p99**    | Extremely slow  | 1 in 100 users     |
| **p99.9**  | Outliers        | 1 in 1,000 users   |

### Why averages are misleading

Example response times (ms):
```
50, 60, 55, 70, 65, 80, 90, 1000
```

Average ≈ 184 ms ❌

p50 = 67 ms ✅

p95 ≈ 1000 ms 😱

Average hides the pain of slow users.

### How percentiles reflect user experience

| Metric | Insight                           |
| ------ | --------------------------------- |
| p50    | “Is the system generally fast?”   |
| p95    | “Are many users affected?”        |
| p99    | “Are rare users suffering badly?” |

### Percentiles and alerts

Good practice:

* Alert on p95 or p99
* Never alert on averages

Example:
```
Alert if p95 > 500 ms for 5 minutes
```

### Important gotchas

1️⃣ Percentiles require enough data

* Low traffic → noisy percentiles
* Use longer windows

2️⃣ Aggregation pitfalls

* You cannot average percentiles
* Percentiles must be calculated from raw data or histograms

3️⃣ Tail latency matters most

p99 usually reveals:

* GC pauses
* lock contention
* cold starts
* slow DB queries

### Percentiles in common tools

#### Prometheus
```
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))
```

#### CloudWatch

* p50 = p50
* p95 = p95
* p99 = p99

#### Datadog / New Relic

* Built-in percentile metrics

### Mental model

* p50 = how fast your system usually is
* p95 = how often users complain
* p99 = how bad the worst experiences are

### Summary

* p50 / p95 / p99 = latency percentiles
* Percentiles show real user experience
* Tail latency matters
* Averages lie
