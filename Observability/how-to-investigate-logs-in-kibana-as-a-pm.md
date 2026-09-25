# How to Investigate Logs in Kibana as a Product Manager

## Make a search reproducible

Velora's support team reports that a food order was charged but appears canceled. Kibana can help inspect recorded evidence, but a search needs a defined data scope, time window and identifier. “I searched and found nothing” is incomplete without those details.

This guide focuses on durable investigation concepts. Kibana layouts, available views and query modes vary by version, deployment and permissions. It does not prescribe a click sequence. Confirm the installed version and use its matching Elastic documentation. The syntax below is **Kibana Query Language (KQL)**, not Lucene, ES|QL or SQL.

## Establish scope before interpreting results

| Check | Record explicitly | Why it matters |
|---|---|---|
| Data scope | Relevant data view or dataset and environment | Production and staging can contain similar-looking IDs |
| Time field | Which timestamp drives the view | Event time and ingestion time answer different questions |
| Window | Absolute start/end and timezone | “Last 15 minutes” changes between reviewers |
| Filters | Query plus active include/exclude filters | An old filter can hide the evidence |
| Fields | Identifier names and mapping types | Text analysis differs from exact keyword matching |
| Visibility | Permissions, retention and ingestion status | No visible result is not proof of no event |

Use only approved data access and redact identifiers when sharing outside the authorized investigation audience.

## Worked search: follow one order, then broaden

Assume this fictional dataset indexes `order.id`, `service.name` and `trace.id` as keyword fields. Do not assume those names or mappings exist in your installation. Verify them with Engineering; adding `.keyword` blindly may reference a nonexistent field.

With the appropriate absolute time window selected, a KQL filter could be:

```text
order.id: "demo-42" AND service.name: "checkout"
```

Inspect timestamps, message meaning, request IDs and outcome fields. Then broaden to the business operation by removing the service restriction deliberately:

```text
order.id: "demo-42"
```

This may reveal payment or fulfillment evidence if those services record the same field. If a matching record exposes a trace ID, it can provide another investigation path. It does not guarantee every related operation was traced or retained. KQL filters records; sorting, aggregation and presentation are separate operations in the surrounding tool.

## Read the result as evidence with limits

Suppose checkout records a timeout while payment records acceptance. That suggests an uncertain customer-facing outcome, not proof of cancellation or completion. Check the authoritative order state through approved tools and clarify what “accepted” means in the payment event contract.

Inspect event time versus ingestion time if the apparent sequence is strange. Sort by the relevant timestamp where the interface supports it, while remembering that clock skew and equal timestamps can limit ordering. A visible sample of rows is not automatically the complete matching population; inspect the view's sampling and result limits before quoting totals.

## Hand off the search, not just a screenshot

Share the approved data scope, exact query, active filters, absolute time window, timezone and a small set of redacted references. Separate observations from hypotheses and name the next check. A saved search or link can help, but access permissions and changing data still affect reproducibility.

Related: [reading logs and correlation IDs](reading-logs-and-correlation-ids-for-pms.md), [incident investigation](production-incident-investigation-for-pms.md) and [percentiles](percentiles-p95-and-p99-for-pms.md) for broader performance evidence.

## Primary references

[Elastic Discover documentation](https://www.elastic.co/docs/explore-analyze/discover) describes the investigation surface. The [KQL reference](https://www.elastic.co/docs/reference/query-languages/kql) explains filtering and field-mapping behavior. These examples are illustrative queries, not a claim of execution against a live Velora or Elastic dataset.

---

### Turn log searches into useful product evidence

**Logs, Kibana, and Observability for Product Managers** — follow Velora’s Kibana and observability scenarios to investigate outcomes without confusing a search result with a complete explanation.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/logs-kibana-observability-product-managers/)
