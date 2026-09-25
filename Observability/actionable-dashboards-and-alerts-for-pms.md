# Actionable Dashboards and Alerts for PMs

## Start with the decision, not the chart

Velora has a dashboard full of checkout charts, but no one knows who should respond when one turns red. A dashboard supports investigation or review. An alert asks someone to act. Neither is useful merely because it displays a technical signal.

Define the customer outcome first: eligible food orders should reach a trustworthy confirmation. Then choose evidence that shows whether that outcome is threatened and who can do something about it.

## A dashboard panel contract

| Field | Illustrative checkout panel |
|---|---|
| Question | Are eligible checkout attempts failing to produce confirmation? |
| Measure | Failed eligible attempts / all eligible attempts in the same window |
| Unit | Attempt, with a separate view of unique affected orders |
| Segments | Platform, payment path and region where available |
| Context | Traffic volume, time window, timezone and recent changes |
| Data quality | Last update, ingestion gaps and known exclusions |
| Next step | Linked investigation view and incident owner |

Repeated retries can inflate attempt counts relative to affected customers. A technical response may succeed while the business operation fails. Agree the event contract with Engineering rather than equating every HTTP 200 with a confirmed order.

## Turn a signal into a response

An alert definition needs a condition, evaluation window, minimum evidence or traffic treatment, severity, recipient, runbook and recovery condition. Choose thresholds with Engineering and service owners based on customer impact and the agreed objective. This guide does not supply a universal threshold.

If a service objective permits 1% unsuccessful eligible operations, an observed 5% failure rate consumes that error allowance at five times the allowed rate **under matching definitions**. This illustrative burn-rate calculation is not an instruction to page at 5%. Traffic volume, duration, remaining budget and multiple observation windows affect the useful response.

A one-request failure in a very quiet service and a sustained failure during peak demand may need different handling. No traffic should not quietly become a healthy 0% error rate if the product was expected to serve requests.

## Worked review: a noisy alert

Velora pages on every payment error. Many errors are recovered retries, so responders repeatedly find no failed orders. Removing all alerts would hide real incidents. Instead, inspect what each alert measures and whether it represents an actionable symptom.

The team might distinguish failed attempts, unresolved orders and provider symptoms, route them appropriately, and group related notifications. Confirm recovery semantics: an alert resolving because traffic stopped does not prove customers can order again. Pair the alert with an explicit journey check and pending-work review.

## Keep the dashboard maintainable

Each panel should have an owner and a defined audience. Remove panels that no longer support a decision; avoid mixing incompatible percentiles or populations just to simplify the display. Restrict high-cardinality identifiers and sensitive drill-down data according to the observability design and access policy.

Review alerts after incidents and repeated false alarms. Test routing and runbook links through approved exercises. A technically accurate condition that wakes nobody—or everyone without ownership—still fails operationally.

Use [the observability glossary](observability-glossary-for-product-managers.md) for SLO vocabulary, [percentiles](percentiles-p95-and-p99-for-pms.md) for latency aggregation, and [Kibana investigation](how-to-investigate-logs-in-kibana-as-a-pm.md) for drill-down evidence.

## Primary reference

[Google SRE: alerting on SLOs](https://sre.google/workbook/alerting-on-slos/) explains error-budget-based alerting and trade-offs between detection and noise.

---

### Connect monitoring with a useful response

**Logs, Kibana, and Observability for Product Managers** — explore Velora’s observability practices and learn which signals support investigation, escalation and customer-impact decisions.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/logs-kibana-observability-product-managers/)
