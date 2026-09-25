# Retention and Cohorts for Product Managers

## Define “returned” before reading the chart

Retention measures whether an eligible population performs a defined return behavior over time. A cohort groups people or accounts using a shared starting condition, such as signup week or first successful project. The starting event, return event, unit and interval all affect the answer.

For Klyvero, suppose the unit is a workspace, the cohort event is its first completed collaborative project, and a return means completing another collaborative task. Merely opening a marketing email would answer a different question.

## Read a cohort table

The illustrative table below uses calendar weeks in an agreed reporting timezone. Week 1 means the calendar week after the cohort week. Denominators stay fixed at the eligible cohort size.

| Cohort | Eligible workspaces | Week 1 returned | Week 2 returned | Week 3 returned |
|---|---:|---:|---:|---:|
| A | 100 | 60 (60%) | 45 (45%) | 40 (40%) |
| B | 80 | 52 (65%) | 36 (45%) | — |
| C | 50 | 35 (70%) | — | — |

A dash means the whole interval has not yet been observed, not zero retention. Compare cohorts at the same age: Week 1 can be compared across all three; Week 3 cannot.

Pooling the observed Week 1 counts gives `(60 + 52 + 35) / (100 + 80 + 50) = 63.9%`, rounded. Averaging the three percentages would give each cohort equal weight despite different sizes. Either approach needs a purpose and a clear label; they are different statistics.

## Several valid definitions, different answers

| Definition | What counts | Interpretation risk |
|---|---|---|
| Return in an exact interval | Activity specifically in Week 2 | Someone returning in Week 3 does not count in Week 2 |
| Return on or after an interval | Activity in Week 2 or any later observed interval | Longer follow-up gives more opportunity to observe a return |
| Consecutive interval retention | Activity in every required interval | Missing one interval can exclude an otherwise active customer |

For exact-interval retention, a later percentage can rise: someone may skip one week and return the next. A retention curve is not inherently required to decline unless its definition imposes that property. Analytics products use different names and settings; inspect the actual definition.

## Match the interval to the product

A weekly planning product should not automatically be judged by daily return. Conversely, a daily operational workflow may need finer-grained analysis. Calendar weeks and elapsed seven-day windows are also different: a Sunday signup has very little opportunity in its first calendar week.

Keep the return event meaningful and stable. If a new background process starts emitting the event without user action, the chart can improve while behavior remains unchanged. If tracking is unavailable during an outage, missing events are not reliable evidence of abandonment.

## A practical investigation sequence

1. Confirm cohort membership, exclusions and the return event.
2. Hide or clearly mark incomplete intervals.
3. Compare the same cohort age across acquisition channels, plans or use cases.
4. Inspect whether the segment mix changed before attributing a change to a release.
5. Combine the pattern with customer research and relevant guardrails.

Higher retention in a new cohort is evidence to investigate, not by itself a causal estimate of an onboarding change. A carefully designed experiment may answer that causal question; a cohort chart alone usually cannot.

See [activation](activation-for-product-managers.md) for first-value hypotheses and [churn](churn-for-product-managers.md) for customer and revenue loss. Retention is not always simply `1 − churn`: the population, interval and events must match.

## Measurement reference

[Amplitude: how retention is calculated](https://amplitude.com/docs/analytics/charts/retention-analysis/retention-analysis-calculation) illustrates how return definitions and time settings change a retention analysis.

## Related resources

- [Retention and Cohort Queries for Product Managers](../SQL/retention-and-cohort-queries-for-product-managers.md) — Calculate mature calendar-week retention with deduplicated returners and fixed cohorts.
- [DAU, WAU and MAU for Product Managers](dau-wau-mau-for-product-managers.md) — Define meaningful activity and distinguish unique audiences from repeated use.

---

### Interpret return behavior in context

**Product Metrics for Product Managers** — follow Klyvero’s measurement decisions and connect retention patterns with the product experience behind them.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/product-metrics-for-product-managers/)
