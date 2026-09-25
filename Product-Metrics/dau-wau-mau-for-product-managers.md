# DAU, WAU and MAU for Product Managers

## Quick Reference

| Measure | Definition once “active” and identity are agreed | What it does not count |
|---|---|---|
| DAU | Distinct active users in a defined day | Number of events or sessions |
| WAU | Distinct active users in a defined seven-day or calendar-week window | Sum of seven daily unique counts |
| MAU | Distinct active users in a defined monthly window | Necessarily the last 30 days; calendar months differ |
| DAU/MAU | Daily unique audience divided by a compatible monthly unique audience | The percentage who returned every day |
| WAU/MAU | Weekly unique audience divided by a compatible monthly unique audience | Cohort retention by itself |

Always label calendar versus rolling windows, timezone and cutoff. A daily window should fall within the monthly window when interpreting DAU/MAU as a share. These examples use calendar days in UTC and stable user IDs; internal users and automated events are excluded consistently.

## Define activity as a product behavior

For Klyvero, meaningful activity might be creating or completing a task, rather than receiving a background notification. Write an event list and explain why it represents engagement. A broader event definition can increase DAU without any change in customer value.

If the decision concerns team adoption, a workspace-based active measure may be more appropriate. Do not divide daily active workspaces by monthly active users. Also define how anonymous users become known users and how shared accounts affect interpretation.

## A small set example

Consider a completed day, its enclosing completed calendar week and its enclosing completed calendar month, all with the same eligible population and activity definition:

```text
Day:   {A, B}          -> DAU = 2
Week:  {A, B, C, D}    -> WAU = 4
Month: {A, B, C, D, E} -> MAU = 5

DAU / MAU = 2 / 5 = 40%
WAU / MAU = 4 / 5 = 80%
```

If the next day's active users are A and C, adding the two daily counts gives four **user-days**, while their combined unique audience is only three people. Repeated activity belongs in some engagement measures, but cannot be silently substituted for a unique audience.

Averaging DAU over the days of a month and dividing by that month's MAU is another metric. Label it explicitly; it is not identical to a selected day's DAU/MAU. Under consistent complete calendar-day definitions, it relates to average active days per monthly active user, not a promise that each person used the product that frequently.

## Interpret cadence, not a universal score

A weekly planning product may create substantial value without daily use. A higher DAU/MAU ratio can also occur when occasional users leave, shrinking MAU. Check numerator and denominator separately and inspect usage frequency or cohort return behavior.

Compare periods with matching completeness and consider holidays, work schedules, product availability and acquisition mix. Rolling windows overlap heavily, so adjacent daily points are not independent observations. An apparent stable trend may partly reflect reuse of the same users in each window.

## Questions for a dashboard review

Which actions count as active? Are all platforms tracked consistently? Are bots or background processes included? Did identity handling change? Does the expected cadence match the job customers use the product for?

Use [activation](activation-for-product-managers.md) for first value, [retention](retention-and-cohorts-for-product-managers.md) for return behavior and [date/time queries](../SQL/date-and-time-queries-for-product-managers.md) for calendar boundaries.

## Measurement reference

[Amplitude's event-segmentation interpretation guide](https://amplitude.com/docs/analytics/charts/event-segmentation/event-segmentation-interpret-1) distinguishes unique users from event totals. Tool-specific “active” defaults should not replace your measurement contract.

---

### Measure engagement at the right cadence

**Product Metrics for Product Managers** — follow Klyvero’s metrics decisions and distinguish meaningful use from larger but less informative counts.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/product-metrics-for-product-managers/)
