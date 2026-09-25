# Logs vs Metrics vs Traces

Which signal should you reach for when a customer says something went wrong?

Velora is a fictional food-ordering platform. A customer says, “I was charged, but my order is canceled.” We need to establish the transaction's outcome and whether other customers are affected. Different signals answer different parts of that investigation.

## A practical comparison

| Signal | What it gives you | Useful question | Important limit |
| --- | --- | --- | --- |
| **Metrics** | Numerical observations and summaries over time. | Has the order-confirmation failure rate increased? | An aggregate may not explain one customer's order. |
| **Logs** | Records of particular occurrences, often with timestamps and identifiers. | What did the system record for order ORD-8472? | Missing or misleading records can leave gaps. |
| **Traces** | Related units of work showing an operation's path and timing. | Which part of this request failed or took the longest? | Uninstrumented or unsampled work may be absent. |

These roles overlap: metrics can be derived from logs, and logs can include trace identifiers. OpenTelemetry explains the relationship in its [signal documentation](https://opentelemetry.io/docs/concepts/signals/).

```text
Order-confirmation problem
    |
    +-- METRICS --> Failure rate over a time window
    |
    +-- LOGS ----> Records for order ORD-8472
    |
    +-- TRACES --> Path and timing of related work
```

These are complementary views, not a required investigation sequence. Connecting a particular order to a trace depends on the identifiers and instrumentation available.

## Follow one case, then establish its scope

### 1. Start with identifiers and time

Get an order ID, the approximate time with its timezone, and the customer's observed outcome. Avoid starting with a broad search for a name when a precise transaction identifier exists.

For this illustrative case, Support gives us `ORD-8472` and a time close to 10:03 UTC. We search a slightly wider window because the remembered time may be approximate. All timestamps below use UTC.

### 2. Reconstruct the recorded sequence

```text
10:03:14  order_id=ORD-8472  order_created
10:03:15  order_id=ORD-8472  payment_id=PAY-3921  payment_requested
10:03:16  order_id=ORD-8472  payment_id=PAY-3921  payment_accepted
10:03:17  order_id=ORD-8472  order_confirmation_failed
10:03:20  order_id=ORD-8472  order_canceled
```

The log suggests a problem after payment acceptance, during order confirmation. It does not establish the root cause or prove that the customer has a settled charge. “Accepted” must be interpreted using the system's event definition and payment records.

The appropriate next question is whether the payment was authorized, captured, reversed or refunded. A financial-state record may answer that more reliably than a generic log message.

### 3. Inspect a related trace, if available

A simplified trace might show:

```text
Confirm order
├── Validate basket       success
├── Request payment       success
└── Save confirmation     error
```

This narrows the investigation to a step. Engineering may still need to inspect the database, dependencies or code to explain why it failed. A failed span is a lead, not a complete diagnosis.

### 4. Check whether the pattern extends beyond this order

Compare order-confirmation failures with eligible confirmation attempts over the same window. Twenty failures out of 100 attempts mean something different from twenty out of 100,000. Then segment by relevant dimensions such as app version or restaurant, without assuming the first segment you find is causal.

The order of investigation can vary. An alert may lead you from metrics to a trace; a support ticket may lead you from logs to a wider metric.

## Hand over evidence, not certainty you do not have

A useful escalation could say:

> ORD-8472 was created around 10:03 UTC. Logs record payment acceptance for PAY-3921, then an order-confirmation failure and cancellation. Please verify the payment's final state and investigate the confirmation step. We have not established the cause or whether other orders are affected.

Include links to the relevant records in the approved internal system. Share only the fields needed for investigation; logs can contain customer information and credentials.

## Questions worth asking Engineering

- Which record is authoritative for the order and payment states?
- Can the order ID connect the logs and trace across services?
- Could sampling, retention or missing instrumentation explain a gap?
- How does this case compare with successful transactions?
- What evidence would distinguish the competing explanations?

For terms such as span, SLO and percentile, see the [observability glossary](observability-glossary-for-product-managers.md).

---

### Practice following the evidence

This resource is part of The Product Shelf's free Technical Product Management library.

**Logs, Kibana, and Observability for Product Managers**

Work through Velora investigations using time windows, related identifiers and log searches, while separating observations from unconfirmed causes.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/logs-kibana-observability-product-managers/)
