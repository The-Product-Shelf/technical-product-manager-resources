# Activation for Product Managers

## Start with a first-value hypothesis

Activation is an operational definition of an early behavior that suggests a user or account has experienced meaningful value. Signing up is easy to instrument, but it may occur before any value is delivered. An activation event is a hypothesis to validate, not proof of future retention.

For Klyvero, a project-management product, consider: **a new workspace creates a project and has a second member complete a task within seven days**. This tries to capture collaboration rather than setup alone. It is illustrative and may be unsuitable for customers who use the product individually.

## Write a measurement contract

| Decision | Example contract | Why it matters |
|---|---|---|
| Unit | Workspace | Avoid mixing users in the numerator with workspaces in the denominator |
| Eligibility | New external workspaces with the collaboration feature available | Internal testing and unavailable functionality should not distort the rate |
| Start | Workspace creation timestamp | Establishes the observation clock |
| Success | Project exists and a different member completes a task afterward | Defines the hypothesized value event and sequence |
| Window | Within seven elapsed days, excluding the upper boundary | Makes opportunity comparable |
| Reporting | Only workspaces whose full window has elapsed | Avoids labeling recent signups as failures |
| Identity | Stable workspace and member IDs | Helps distinguish a second person from repeated events |

Agree details with Data: timezone, merged accounts, event delivery delays and whether imported tasks count. Keep the definition versioned when it changes so a chart does not silently mix different measures.

## Worked calculation

A signup cohort contains 120 workspaces. Twenty have not yet had seven days of opportunity, leaving 100 mature workspaces. Of those, ten are excluded under the agreed internal-account rule, leaving 90 eligible workspaces. Thirty-six satisfy the full activation condition.

```text
120 workspaces
 -20 incomplete observation windows
 -10 ineligible workspaces among the mature group
 =90 eligible, fully observed workspaces

36 activated / 90 eligible = 40%
```

Do not subtract the same workspace twice if exclusions overlap. This example applies the filters sequentially to make that explicit. Reporting `36 / 120` would answer a different question and understate this defined rate.

## Validate the hypothesis

Compare later meaningful retention for activated and non-activated workspaces using cohorts with comparable observation time. Check customer segment and acquisition differences. Customers with higher initial intent may both activate and retain, so an association does not establish that forcing the activation action causes retention.

Use interviews or session evidence to understand whether the event actually reflects value. If people complete tasks only to dismiss onboarding prompts, the metric may be easy to increase without improving the product. Consider whether small and large teams need different first-value hypotheses rather than forcing one threshold on everyone.

## Use the metric to diagnose a journey

Break the path into observable steps: project created, colleague invited, invitation accepted, task completed. Inspect where eligible workspaces stop and how long successful journeys take. Invitation acceptance depends on another person; repeated reminder prompts may increase messages while harming trust.

Pair activation with guardrails such as unwanted invitation reports and support contacts. The specific measures and acceptable levels depend on the product; the numbers here are not benchmarks.

## Questions for the next review

- Which evidence connects this event to customer value?
- Can a customer get value without satisfying the definition?
- Does every eligible workspace have access and sufficient time?
- Did the behavior improve, or did tracking or eligibility change?
- Which segment should we investigate before changing onboarding?

Use [funnel queries](../SQL/product-funnel-queries-for-product-managers.md) to implement ordered steps and [retention cohorts](retention-and-cohorts-for-product-managers.md) to evaluate later behavior. The [metrics cheat sheet](product-metrics-cheat-sheet.md) provides complementary definitions.

## Related resources

- [Funnel Analysis for Product Managers](funnel-analysis-for-product-managers.md) — Interpret drop-offs and choose an investigation without confusing friction with tracking gaps.

---

### Connect early behavior with lasting value

**Product Metrics for Product Managers** — explore how Klyvero chooses product measures, interprets changes and avoids optimizing isolated numbers.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/product-metrics-for-product-managers/)
