# Progressive Rollouts for Product Managers

## Expand exposure only when the evidence supports it

A progressive rollout introduces a change to a limited population before expanding exposure. It can reduce the number of people exposed to an undetected problem, but it cannot guarantee safety. The initial group may miss rare conditions, peak load or dependencies shared with everyone.

For Lumen, consider a new lesson-completion flow. The useful plan is more specific than “release to 5%”: five percent of what, selected how, observed for how long, and stopped by whom?

## Write a rollout card

| Decision | Illustrative plan | Why it matters |
|---|---|---|
| Exposure unit | Stable learner account assignment | One person should not switch behavior on every request unexpectedly |
| Eligibility | Supported app versions and lesson types | Excludes clients unable to handle the new flow |
| First audience | A defined small cohort | Internal testing alone may not represent real learners |
| Technical evidence | Completion errors, latency and missing progress updates | Detects failure beyond the interface |
| Product guardrails | Duplicate completions and support reports | Catches harmful behavior despite successful responses |
| Observation window | Covers the relevant learning and sync cycle | Ten quiet minutes may miss delayed sync failures |
| Stop owner | Named release decision-maker with Engineering support | Enables an accountable response |
| Recovery | Validated flag behavior plus data-repair plan if needed | Hiding the feature does not undo stored changes |

All percentages, windows and thresholds must be chosen for the actual risk and traffic. There is no universal safe first cohort size.

## Understand the mechanisms

A canary deployment sends limited traffic or workloads to a new version while the rest use a baseline. A feature flag can control access to behavior within deployed software. They can be combined, but they are not synonymous: the infrastructure routing unit may differ from the product's audience unit.

```text
Deploy and validate
  -> limited eligible audience
  -> review evidence
  -> expand, hold, or stop
  -> review again before wider exposure
```

A rollout is not automatically an experiment. Random assignment, suitable controls and analysis can support causal inference, but a staged release chosen for operational safety may not meet those conditions.

## Interpret the first cohort carefully

Suppose the new flow shows fewer errors than the baseline, but it was enabled only for recent devices in one region. That comparison cannot establish a universal improvement. Inspect comparable segments, request volume and data completeness before expanding.

Also look for shared effects. If the new flow overloads a shared progress service, learners outside the rollout may suffer too. A small exposure percentage does not necessarily cap the blast radius at that percentage.

Lumen should verify that assignment remains consistent across devices and that older clients can still read progress written by newer ones. Cross-device behavior can otherwise make the experience confusing even when each individual request succeeds.

## Stop conditions need executable responses

Describe what evidence triggers a pause and what the team will do. A flag may stop new use but leave queued work, already-written data or ongoing sessions. A rollback may require backward-compatible data and APIs. Confirm these constraints before the release, not during an incident.

After wider exposure, keep watching the full population and remove obsolete flags through the team's normal process. “Reached 100%” is not proof that monitoring or cleanup is complete.

Related: [deployment, release, rollback and flags](deployment-release-rollback-feature-flags.md), [mobile releases](mobile-app-releases-for-product-managers.md), and [incident investigation](../Observability/production-incident-investigation-for-pms.md).

## Operational reference

[Google SRE: canarying releases](https://sre.google/workbook/canarying-releases/) explains evaluation and operational considerations for limited initial exposure.

## Related resources

- [Release Dependencies and Readiness for PMs](release-dependencies-and-readiness-for-pms.md) — Coordinate compatible components, rollout gates and recovery ownership.
- [Actionable Dashboards and Alerts for PMs](../Observability/actionable-dashboards-and-alerts-for-pms.md) — Connect a signal to a customer outcome, response owner and useful next action.

---

### Plan releases around evidence and recovery

**CI/CD and releases for Product Managers** — follow Lumen’s delivery scenarios to understand how pipelines, deployment and customer exposure fit together.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/ci-cd-and-releases-for-pms/)
