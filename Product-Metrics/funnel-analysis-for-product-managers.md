# Funnel Analysis for Product Managers

## Use a drop-off to choose the next investigation

Klyvero's onboarding funnel shows fewer people inviting a teammate than creating a project. That does not yet identify a usability problem. Some users work alone, invitations can happen later, and a missing event can look like abandonment. A funnel locates an observed pattern; investigation explains it.

This guide concerns interpretation. The [SQL funnel guide](../SQL/product-funnel-queries-for-product-managers.md) provides a separate executable implementation with ordering and time-window rules.

## Agree the analysis contract

Specify the unit—person or workspace—the entry event, eligibility, ordered steps, completion window and identity rules. Decide whether unrelated actions may occur between required steps, whether repeated entry resets the clock, and how timestamp ties are treated. Analytics tools offer different settings; two charts with the same labels may count different journeys.

For this illustrative Klyvero analysis, count eligible new users with a full seven-day window after their first setup start. Require project creation after starting and invitation after creation; unrelated actions are allowed. Exclude internal test users using a documented rule.

| Step | Users reaching it | Previous-step conversion | Conversion from entry |
|---|---:|---:|---:|
| Start setup | 200 | — | 100% |
| Create project | 120 | 60% | 60% |
| Invite teammate | 60 | 50% | 30% |

Eighty users do not reach creation; sixty reach creation but not invitation. The last step loses a larger *share* of its entrants, while the earlier step loses more *people*. Both views matter when choosing where to investigate.

## Check whether the funnel measures the intended value

An invitation is not acceptance, collaboration or retained use. If Klyvero's hypothesis is “a team completes work together,” increasing invitation sends alone can be a weak proxy. Link the funnel to an [activation definition](activation-for-product-managers.md) and later [retention](retention-and-cohorts-for-product-managers.md), without assuming correlation establishes causation.

Also separate intended audiences. A solo user declining an invitation step may be making a valid choice. Moving the invitation earlier could raise sends while reducing trust or increasing unwanted messages. The desired path should follow the product's value proposition rather than a wish for every bar to reach 100%.

## A practical diagnosis sequence

1. Verify event delivery and identity stitching around the apparent drop-off.
2. Compare fully observed populations with the same entry and completion rules.
3. Segment by relevant opportunity, such as team intent, plan, platform or feature availability.
4. Inspect completion-time distributions; successful users may need longer than expected.
5. Pair the pattern with customer research or observed journeys before selecting a change.

Do not sum overlapping segments, and watch small denominators. A platform split can reveal a missing mobile event rather than a mobile UX failure. A recent cohort can look worse simply because its full window has not elapsed.

## Turn the finding into a testable next step

A useful conclusion is “Among team-intent users with seven days of observation, invitation completion is lower on mobile; first verify the event and inspect those journeys.” It names evidence and uncertainty. “Users hate invitations” goes beyond the chart.

Record the expected mechanism for any change, its success measure and guardrails. Use [metric-change investigation](evaluating-a-metric-change-for-product-managers.md) when evaluating the result.

## Primary reference

[Amplitude's funnel calculation documentation](https://amplitude.com/docs/analytics/charts/funnel-analysis/funnel-analysis-how-amplitude-computes-conversions) illustrates how ordering and conversion settings affect results; confirm your own tool's settings.

---

### Connect conversion patterns with product decisions

**Product Metrics for Product Managers** — explore Klyvero’s funnels, activation and measurement choices while keeping the customer outcome in view.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/product-metrics-for-product-managers/)
