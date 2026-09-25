# Reading Logs and Correlation IDs for PMs

## Start with an event, not an error count

A log is a recorded observation from part of a system. To investigate Velora's food-order checkout, begin with the customer's action, approximate time and a permitted transaction reference. A large number of error lines does not directly tell you how many customers were affected.

Use approved observability access. Share redacted references rather than payment information, access tokens, personal data or full customer payloads. Even identifiers can be sensitive when they connect to customer records.

## Know which identifier you are following

| Identifier | Typical purpose | What not to assume |
|---|---|---|
| Order or booking ID | Follow one business object over its lifetime | Every attempt has a different order |
| Request ID | Follow a particular request | All retries share the same request ID |
| Trace ID | Connect spans in a traced operation | Every service is instrumented or every trace is retained |
| Correlation ID | Link related activity under an agreed convention | Its scope is standardized across every system |
| Idempotency key | Recognize repeated attempts at one intended operation | It is interchangeable with any tracing ID |

Actual fields and propagation depend on the implementation. A request can produce several downstream operations, and an asynchronous worker may continue after the original request ends. Ask Engineering which fields reliably bridge those boundaries.

## Worked timeline: the retry is not a second purchase

The following entries are invented and deliberately omit personal and payment details. Times represent event timestamps in UTC, not ingestion time.

```text
10:00:00  checkout  order=demo-42 request=r1  submit started
10:00:02  payment   order=demo-42 request=p1  payment accepted
10:00:03  gateway   order=demo-42 request=r1  response timed out
10:00:06  checkout  order=demo-42 request=r2  retry received
10:00:06  checkout  order=demo-42 request=r2  existing result returned
```

This supports the hypothesis that the payment succeeded before the customer-facing request timed out. It does not prove the restaurant received the order, the user saw confirmation, or every customer had the same outcome. Check the authoritative order state and fulfillment evidence through approved tools.

Searching only for `request=r1` could miss the retry and payment call. Searching for the business reference may connect them, provided services actually record that reference. The phrase “payment accepted” also needs an event contract: authorization, capture and settlement are different payment states.

## Build an evidence trail

1. Record the time range and timezone; widen it enough to include preceding and asynchronous activity.
2. Filter by a known reference and relevant service, then inspect surrounding events.
3. Separate attempts from unique business operations before estimating impact.
4. Compare event time with ingestion time if logs arrived late.
5. Record what is observed, what is inferred and what remains unknown.

Clock differences between services can make timestamp order misleading. A trace's parent-child relationships can add context, but missing instrumentation or sampling may still leave gaps. Do not conclude “this never happened” just because a search has no result; retention, permissions, filters and dropped records can all affect visibility.

## Common interpretation traps

An `ERROR` level is a producer's classification, not a universal severity scale. A successful retry can leave error logs while the customer operation succeeds. Conversely, a business failure may occur without an exception: a rule can reject a valid-looking checkout and return a technically successful response.

Use [logs, metrics and traces](logs-vs-metrics-vs-traces.md) together. [Timeouts and retries](../APIs/api-timeouts-and-retries-for-product-managers.md) explain uncertain outcomes; [incident investigation](production-incident-investigation-for-pms.md) turns a local timeline into an impact and response conversation.

## Technical reference

[OpenTelemetry: traces](https://opentelemetry.io/docs/concepts/signals/traces/) explains trace and span relationships and context propagation.

---

### Follow the evidence behind a customer problem

**Logs, Kibana, and Observability for Product Managers** — work through Velora’s observability scenarios and learn how logs and Kibana support product investigation.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/logs-kibana-observability-product-managers/)
