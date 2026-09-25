# Product Metrics Cheat Sheet

A reference for defining metrics before putting them on a dashboard.

Klyvero is a fictional project management product. Its users create projects, organize tasks and invite teammates. The definitions below are examples for discussion, not universal standards or recommended performance benchmarks.

## Give every metric a clear definition

Before calculating a rate, record its **unit** (user, account or event), **eligible population**, **qualifying behavior**, **time window**, **timezone** and **exclusions**. Name an owner who can explain changes to the definition.

For example: “The percentage of new eligible accounts created in August that invite a teammate within seven days of account creation, using UTC and excluding internal test accounts.” Wait until each account has had its full seven days before treating the cohort as complete.

## Common metrics and the choices behind them

| Metric | Example calculation | What to clarify |
| --- | --- | --- |
| **Sign-up conversion** | Visitors who sign up ÷ eligible visitors × 100 | Identity matching, conversion window, and whether repeated visits count once. |
| **Activation rate** | New accounts completing the agreed first-value behavior within seven days ÷ eligible new accounts with seven days of observation × 100 | Why the behavior is evidence of value; completion must occur within the account's own window. |
| **Feature adoption** | Eligible active users using Templates at least once in the month ÷ eligible active users in that month × 100 | Eligibility, access and the definition of active; the numerator must belong to the denominator. |
| **Usage frequency** | Template uses in the month ÷ users who used Templates in that month | This describes adopters only. Also examine the distribution, because a few heavy users can dominate the average. |
| **Week-4 retention** | Eligible users from a starting cohort performing the qualifying action in week 4 ÷ eligible users in that cohort × 100 | For this example, week 4 means days 28–34 after signup; other conventions exist. |
| **Monthly customer churn** | Paying accounts present at month start that leave by month end ÷ paying accounts at month start × 100 | Define “leave,” and how cancellations, pauses and reactivations are handled. |
| **Average revenue per paying account** | Revenue for the period attributed to a defined paying-account population ÷ number of accounts in that population | Keep the revenue basis, currency, period and account population aligned. |
| **Funnel step conversion** | Eligible entities reaching the next step in order and within the allowed window ÷ entities entering this step × 100 | Separate events are not automatically evidence that the same entity completed the sequence. |

Definitions vary between analytics systems. For an example of how retention settings change interpretation, see [Amplitude's retention documentation](https://amplitude.com/docs/analytics/charts/retention-analysis/retention-analysis-calculation).

## Example: more uses, fewer adopters

Suppose these are Klyvero's recorded results for two comparable months. All values are illustrative.

| Measure | Month A | Month B |
| --- | --- | --- |
| Eligible active users | 1,000 | 1,000 |
| Users using Templates | 200 | 150 |
| Template uses | 800 | 900 |
| Adoption | 20% | 15% |
| Uses per adopting user | 4 | 6 |

Total usage increased while adoption fell. Existing adopters may be using Templates more frequently, or the mix of adopters may have changed. These totals do not tell us which explanation is correct.

If the goal was broader discovery, the rise from 800 to 900 uses is insufficient evidence of success. We should inspect exposure, new versus returning users, and the distribution of uses per person before choosing a response.

## Comparisons that deserve a second look

A conversion change from 20% to 25% is **5 percentage points**, or a **25% relative increase**. State which one you mean.

Compare cohorts with equal opportunity to complete the behavior. Recent signups cannot yet demonstrate week-4 retention. An incomplete current month should not be compared casually with a complete previous month.

A before-and-after difference is evidence of change, not proof that a release caused it. Acquisition mix, seasonality, pricing or tracking changes may also explain the movement. Work with Data on an appropriate comparison or experiment.

## Questions worth asking Data

- Can we write the numerator and denominator in one unambiguous sentence?
- Does every entity have the full observation window?
- Did tracking, eligibility or identity resolution change?
- Which segments or distributions might the aggregate hide?
- What decision would a higher or lower number change?

Use [North Star, Input Metrics & Guardrails](north-star-input-and-guardrail-metrics.md) to connect these definitions to a product goal, and the [SQL cheat sheet](../SQL/sql-cheat-sheet-for-product-managers.md) to begin exploring recorded activity.

---

### Want to go deeper?

This resource is part of The Product Shelf's free Technical Product Management library.

**Product Metrics for Product Managers** explores these topics through practical product scenarios.

→ [Explore the book at The Product Shelf](https://theproductshelf.com/product/product-metrics-for-product-managers/)
