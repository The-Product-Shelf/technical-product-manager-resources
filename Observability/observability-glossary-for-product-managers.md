# Observability Glossary for Product Managers

Vocabulary for understanding product health and gathering useful evidence when something goes wrong.

Velora is a fictional food-ordering platform. Support reports that a customer was charged but sees a canceled order. Observability helps us investigate the system's behavior using the signals it produces. It does not guarantee that every action was recorded or that a single error reveals the cause.

## From behavior to evidence

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Telemetry** | Data emitted about a system's activity and condition. | Investigations depend on what was actually recorded. |
| **Instrumentation** | The work that makes software emit useful signals. | A new feature may need new evidence to explain its behavior after launch. |
| **Event** | A record that something happened. | A click, payment confirmation and failed request describe different stages. |
| **Log** | A timestamped record of an occurrence. | It can help reconstruct what happened to a particular order. |
| **Metric** | A numerical measurement, often summarized over time. | It can show whether an issue is widespread or changing. |
| **Trace** | A representation of an operation's path through instrumented work. | It can show where time was spent or a failure appeared. |
| **Span** | A recorded unit of work within a trace. | One slow step can matter even when neighboring steps are fast. |
| **Correlation ID** | An identifier used to connect related records. | Share the relevant order, request or trace identifier when escalating. |
| **Dashboard** | A selected view of measurements or records. | Its filters and omissions determine what you can see. |
| **Alert** | A notification triggered by a defined condition. | Decide who responds and what action the signal warrants. |
| **Incident** | A disruption requiring coordinated response. | Communicate the impact and current evidence without guessing a cause. |

For how the signals relate, see [OpenTelemetry's signal concepts](https://opentelemetry.io/docs/concepts/signals/) and the companion [Logs vs Metrics vs Traces](logs-vs-metrics-vs-traces.md).

## Reliability terms

| Term | Meaning | Product implication |
| --- | --- | --- |
| **SLI — service level indicator** | A defined measure of service behavior. | Specify the journey, eligible events and measurement point. |
| **SLO — service level objective** | A target for an SLI over a window. | Agree on an acceptable experience using a measurable target. |
| **SLA — service level agreement** | An agreement about service levels and their consequences. | Do not assume an internal target is a customer commitment. |
| **Error budget** | The amount of unreliability allowed by an SLO. | It can inform discussions about reliability work and release risk. |
| **Percentile** | A value at or below which a stated proportion of observations fall. | p95 latency describes the slower end better than an average alone. |
| **Sampling** | Recording or retaining a subset of observations. | An absent trace may reflect collection policy. |
| **Retention** | How long telemetry remains available. | Older incidents may be impossible to reconstruct from expired records. |

See Google's [service level objectives chapter](https://sre.google/sre-book/service-level-objectives/) for the reliability terminology.

## Example: turn “checkout should be reliable” into a question

Suppose Velora proposes an illustrative SLO: 99.9% of eligible order-confirmation attempts should succeed over a rolling 30-day window. First define eligibility. Are invalid orders excluded? Does success require a saved order, a payment result, or a confirmation visible to the customer?

If the measured window contains 100,000 eligible attempts, a 99.9% target allows 100 unsuccessful attempts. This is a request-based allowance, not automatically a number of minutes of downtime. The same count can also hide concentrated harm to one restaurant or customer group.

Latency needs a separate definition. If p95 is two seconds, about 95% of measured requests complete within that value; the slowest requests may take much longer. Ask whether the measure covers the full customer wait or only one backend step.

The target is a proposal for discussion, not a benchmark for Velora or every checkout.

## Questions worth asking Engineering

- Which user-visible outcome does this indicator measure?
- What is excluded, sampled or missing from the telemetry?
- Can we connect a support case to the corresponding operation?
- Who receives an alert, and what should happen next?
- Are we viewing the same time window, timezone and customer segment?

---

### Turn observability terms into an investigation

This resource is part of The Product Shelf's free Technical Product Management library.

**Logs, Kibana, and Observability for Product Managers**

Follow Velora’s transactions and learn how timestamps, identifiers and Kibana searches help you gather useful incident context.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/logs-kibana-observability-product-managers/)
