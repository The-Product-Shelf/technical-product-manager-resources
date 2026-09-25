# Percentiles, p95 and p99 for PMs

## What a percentile says

A latency percentile describes a position in a measured distribution. Roughly, p95 is the value at or below which 95% of measured requests fall; p99 focuses farther into the slow tail. The exact calculation depends on the quantile method and, in monitoring systems, may be estimated from histogram buckets.

Neither number is a maximum, a promise to every user, nor automatically a measure of complete user journeys. First identify what was timed.

## An intentionally uneven example

Velora measures 100 checkout API requests:

| Requests | Latency per request |
|---|---:|
| 90 | 100 ms |
| 8 | 500 ms |
| 2 | 5,000 ms |

Using the **nearest-rank** method—sort observations and take rank `ceil(p × N)`—the results are:

| Summary | Result | What it reveals |
|---|---:|---|
| Mean | 230 ms | Total latency divided by 100 |
| Median / p50 | 100 ms | The middle observation is fast |
| p95 | 500 ms | Rank 95 lies in the slower group |
| p99 | 5,000 ms | Rank 99 reaches the very slow group |
| Maximum | 5,000 ms | Slowest observation in this sample |

The average looks modest even though two requests took five seconds. These are illustrative values, not acceptable-performance benchmarks. Another percentile interpolation method may produce different results on small samples.

## Ask what the timer includes

```text
User taps checkout
  -> device/network delay
  -> API processing
  -> payment dependency
  -> response reaches device
  -> interface confirms outcome
```

An API dashboard may measure only a subset of this path. A fast server response can coexist with a slow interface. A checkout journey may include several requests, so the p95 of one endpoint is not the p95 of the entire journey. You also cannot generally add per-service p95 values to obtain an end-to-end p95.

Timeouts and failed requests require special attention. If the chart includes only successful responses, the worst experiences may disappear from the latency distribution. Pair latency with failure rates and clearly labeled populations.

## Avoid invalid aggregation

Do not average the p95 values of two servers to obtain the combined p95. Their traffic volumes and distributions may differ, and a percentile alone does not retain enough information to reconstruct the combined distribution. Ask Engineering whether the monitoring system can aggregate compatible histogram data correctly.

The same issue applies to averaging five-minute p95 values into a daily p95. A daily distribution and an average of short-window summaries answer different questions. Sparse traffic also makes tail estimates unstable: p99 over a tiny sample offers little evidence about rare behavior.

## Use percentiles in a product decision

If checkout p95 rises, inspect the measurement window, traffic volume and relevant segments: region, device, app version, payment path or account type. A change in segment mix can move the overall number without any segment getting slower.

Compare against an agreed user-facing objective and examine whether slow requests also fail or trigger repeated purchases. A percentile improvement matters when it reflects a better experience for the intended population, not merely a change in excluded requests.

Use [correlation IDs](reading-logs-and-correlation-ids-for-pms.md) to investigate representative slow attempts and the [observability glossary](observability-glossary-for-product-managers.md) for service-level measures.

## Technical reference

[Google SRE: monitoring distributed systems](https://sre.google/sre-book/monitoring-distributed-systems/) discusses latency distributions, tail behavior and the limitations of averages.

---

### Read performance charts as customer evidence

**Logs, Kibana, and Observability for Product Managers** — follow Velora’s observability examples to connect technical signals with the experience users actually receive.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/logs-kibana-observability-product-managers/)
