# Churn for Product Managers

## Quick Reference

| Measure | Illustrative period formula | What it tells you |
|---|---|---|
| Customer churn | Lost starting customers / starting customers | How many existing customers left |
| Gross revenue churn | (Lost recurring revenue + contraction) / starting recurring revenue | Existing revenue lost before expansion |
| Net revenue churn | (Lost revenue + contraction − expansion) / starting recurring revenue | Loss after expansion in the starting population |
| Net revenue retention (NRR) | (Starting revenue − lost revenue − contraction + expansion) / starting revenue | Revenue retained and expanded in that population |

Use a consistent recurring-revenue measure such as MRR. New customers acquired during the period do not improve retention of the starting population. Definitions for reactivation, unpaid accounts and cancellation timing must be documented with Finance and Data.

## A ledger you can reconcile

Klyvero begins a month with four paying workspaces. For this simplified example, recurring revenue amounts are normalized monthly values in the same currency; there are no exchange-rate changes, credits or reactivations.

| Starting workspace | Starting MRR | Ending MRR | Movement |
|---|---:|---:|---|
| Alpha | 100 | 0 | Cancellation: 100 lost |
| Beta | 200 | 150 | Contraction: 50 lost |
| Gamma | 300 | 400 | Expansion: 100 gained |
| Delta | 400 | 400 | No change |
| **Starting population total** | **1,000** | **950** | **Net loss: 50** |

A fifth workspace, Epsilon, joins during the month with MRR of 200. Total ending MRR is now 1,150, but starting-population retention is still calculated from the four original workspaces.

- Customer churn: `1 / 4 = 25%`.
- Gross revenue churn: `(100 + 50) / 1,000 = 15%`.
- Net revenue churn: `(100 + 50 − 100) / 1,000 = 5%`.
- NRR: `950 / 1,000 = 95%`.

Total MRR grew by 15% because of new business despite a net loss from existing customers. Growth does not prove existing-customer retention improved. Expansion also cannot reduce **gross** revenue churn, because that measure intentionally excludes it.

## Interpret the numbers carefully

Net revenue churn can be negative, corresponding to NRR above 100%, when expansion exceeds lost revenue and contraction. That does not mean no customers left. A small number of large expansions can hide a broad pattern of smaller customers leaving.

Customer churn treats a small and a large account equally. Revenue churn weights their economic contribution. Both can be useful; neither explains the cause of cancellation on its own.

MRR is not cash collected or accounting revenue recognized in the month. Annual contracts typically need normalization, and billing-system settings determine how discounts, delinquency and other adjustments enter reporting. Reconcile the metric definition before comparing dashboards.

## Decisions that need an explicit policy

- Does a customer count as lost when cancellation is requested or when paid access ends?
- Is the customer a workspace, billing account or parent company?
- How are pauses, non-payment and involuntary payment failures handled?
- How does a same-period cancellation and reactivation affect movements?
- Are starting and ending amounts calculated in a consistent currency?

A zero starting population or zero starting MRR makes the relevant percentage undefined. Do not replace that with a measured 0% churn. Small populations can also produce large swings from one cancellation.

Use [retention cohorts](retention-and-cohorts-for-product-managers.md) to inspect tenure and segment differences, and the [metrics cheat sheet](product-metrics-cheat-sheet.md) for related business measures. Investigate cancellation reasons alongside actual behavior; survey answers can be incomplete.

## Measurement reference

[Stripe Billing: subscription analytics](https://docs.stripe.com/billing/subscriptions/analytics) documents billing metrics and configurable definitions. Its implementation is an example, not a universal reporting standard.

---

### Separate growth from existing-customer health

**Product Metrics for Product Managers** — explore Klyvero’s product and business metrics with attention to populations, trade-offs and misleading averages.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/product-metrics-for-product-managers/)
