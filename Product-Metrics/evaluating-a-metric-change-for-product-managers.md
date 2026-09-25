# Evaluating a Metric Change for Product Managers

## Start with a claim you can check

Klyvero's activation rate falls after a release. Before concluding that onboarding became worse, check what was measured, who was eligible and whether the cohorts had equal opportunity. A before/after movement can be real without being caused by the release.

This guide is a triage sequence for a metric change, not a statistical significance test or a substitute for experimental design.

## Check four layers

| Layer | Evidence to inspect | Example alternative explanation |
|---|---|---|
| Measurement | Event definitions, delivery, joins and identity rules | A client stopped emitting the activation event |
| Population | Eligibility, cohort maturity and segment mix | More new accounts are still inside their observation window |
| Behavior | Comparable segment rates and customer journeys | One platform has a genuine completion problem |
| Attribution | Assignment, concurrent changes and uncertainty | A campaign changed the audience at the same time as the release |

Keep numerator and denominator visible. A percentage without its counts makes both data errors and small-sample swings harder to recognize.

## Worked example: every segment improves, the total falls

Suppose all workspaces have complete observation windows and unchanged eligibility. “Team-intent” and “solo-intent” are mutually exclusive, stable classifications recorded at entry.

| Period | Segment | Eligible workspaces | Activated | Rate |
|---|---|---:|---:|---:|
| Before | Team-intent | 80 | 64 | 80% |
| Before | Solo-intent | 20 | 4 | 20% |
| After | Team-intent | 20 | 17 | 85% |
| After | Solo-intent | 80 | 20 | 25% |

The total changes from `68 / 100 = 68%` to `37 / 100 = 37%`: a fall of **31 percentage points**, or approximately **45.6% relative** to 68%. Yet each segment improves by five percentage points. The shift toward the lower-rate segment drives the aggregate decline.

Holding the earlier 80/20 mix fixed would yield `0.8 × 85% + 0.2 × 25% = 73%`. That standardized comparison helps describe the mix effect. It does not prove the release caused a five-point improvement, because other influences or sampling variation may remain.

## Investigate before selecting a response

First reconcile a sample of underlying records and the metric definition with Data. Check whether the apparent change coincides with instrumentation, identity or eligibility changes. Use [duplicate and NULL diagnosis](../SQL/finding-duplicates-and-handling-nulls-for-pms.md) when counts behave unexpectedly.

Then compare like with like: cohort age, platform, plan, acquisition source and natural usage cadence. Avoid searching hundreds of segments until one looks dramatic; state why the segment is relevant, and treat exploratory findings as hypotheses needing confirmation.

Ask for uncertainty estimates appropriate to the design and unit of analysis. Multiple events from the same workspace are not automatically independent observations. A short noisy window or repeated peeking can support more confidence than the evidence warrants if analyzed carelessly.

## Decide what the evidence supports

A tracking break may require repairing measurement before judging the feature. A clear customer failure may justify mitigation without waiting for a causal estimate. A business-outcome claim may require an experiment or a more careful observational design agreed with Data.

Record what changed, the best-supported explanation, remaining uncertainty and the next decision. Avoid treating “not statistically significant” as proof of no effect, or a significant association as proof of causation.

Related: [activation](activation-for-product-managers.md), [retention cohorts](retention-and-cohorts-for-product-managers.md), [funnels](funnel-analysis-for-product-managers.md) and [North Star inputs and guardrails](north-star-input-and-guardrail-metrics.md).

## Primary reference

[Microsoft Research: metric interpretation pitfalls](https://www.microsoft.com/en-us/research/publication/a-dirty-dozen-twelve-common-metric-interpretation-pitfalls-in-online-controlled-experiments/) examines how misleading metric movements can produce incorrect decisions. The synthetic mix-shift example above is a diagnostic illustration, not an experiment result.

---

### Interpret movement before celebrating or reacting

**Product Metrics for Product Managers** — explore Klyvero’s measurement choices and connect definitions, evidence and uncertainty with better product decisions.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/product-metrics-for-product-managers/)
