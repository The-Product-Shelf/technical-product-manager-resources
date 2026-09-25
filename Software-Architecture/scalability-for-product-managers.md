# Scalability for Product Managers

## Translate growth into a workload

Nómada plans a campaign for its online shop. “Support ten times more users” is not yet a useful capacity requirement: ten times more browsing, checkouts, large exports and concurrent sessions can stress different parts of the system.

Scalability concerns how a system handles changing load while meeting agreed objectives. A faster response for one customer is not proof that the system can sustain a larger workload. More infrastructure can help, but shared dependencies or inefficient work may remain the limiting factor.

## A PM's capacity brief

| Input | Illustrative Nómada assumption | Why Engineering needs it |
|---|---|---|
| Arrival pattern | 6,000 visits in ten minutes after an email | A burst differs from the same volume spread over a day |
| Journey mix | Browsing, search and checkout proportions | Each journey performs different work |
| Concurrency | Estimated overlapping requests and sessions | Registered accounts are not concurrent operations |
| Data shape | Large baskets and popular limited-stock products | Average-sized examples can hide contention |
| Objective | Agreed latency and success rate for checkout | Throughput alone does not describe customer experience |
| Duration and recovery | Burst length and acceptable backlog drain time | The system must recover after the peak too |

If each of 6,000 visits triggers eight backend requests, that is 48,000 requests over 600 seconds: **80 requests per second on average**. It is not the peak, and it excludes retries, background jobs and changes in user behavior. Validate the request-per-visit assumption with Engineering; cached requests might not reach every backend.

## Compare options against the bottleneck

Scaling up adds capacity to a component; scaling out distributes work across more instances. Either can be appropriate. More application instances will not automatically fix a saturated shared database or a provider's fixed rate limit.

Other options include reducing unnecessary work, caching suitable data, controlling admission or moving non-urgent work to a queue. Each changes costs or behavior. A queue can delay receipt emails without delaying checkout, but cannot promise instant fulfillment during a sustained backlog. See [queues](queues-and-message-brokers-for-pms.md) and [caching](caching-for-product-managers.md).

## Ask for a test that represents the campaign

A useful test report names the environment, dataset, workload mix, ramp-up, duration and dependencies that were simulated. It reports latency distributions, success rates and saturation alongside throughput. A short test with tiny baskets may not justify a promise about a long sale with hot inventory records.

Compare the measured operating region with the forecast and its uncertainty. Agree what happens if demand exceeds it: queue admission, a truthful waiting state, limited nonessential features or another reviewed behavior. Thresholds should come from the system and customer need, not a generic benchmark.

## Include cost and recovery

Automatic scaling may have startup delays, limits and cost consequences. A sudden burst can arrive before extra capacity is ready. When traffic falls, check that backlogs drain and dependencies recover; a normal request rate does not imply pending work is complete.

Ask who owns the forecast, what assumption most threatens it, and what evidence would change the launch plan. No architecture label guarantees capacity, and microservices are not a prerequisite for growth.

Use [environment parity](../CI-CD/environment-parity-for-product-managers.md) to interpret test limits and [percentiles](../Observability/percentiles-p95-and-p99-for-pms.md) to read tail latency.

## Primary reference

[Microsoft's capacity-planning guidance](https://learn.microsoft.com/en-us/azure/well-architected/performance-efficiency/capacity-planning) connects workload assumptions, performance objectives and resource planning.

---

### Turn growth forecasts into technical questions

**Software Architecture for Product Managers** — explore how Nómada’s architecture responds to product demand, constraints and operating trade-offs.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
