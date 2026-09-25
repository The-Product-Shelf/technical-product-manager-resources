# Library Coverage and Editorial Roadmap

Technical skills for Product Managers, explained simply.

This is a maintenance-oriented **candidate coverage map**, not a publication quota or commitment. The current library contains **56 resources across seven categories**. Future additions depend on a distinct reader need and evidence that existing pages cannot answer it well.

## Re-audit before the second batch

The audit used current main after PR #2: README, CONTRIBUTING, ROADMAP and all 38 resources. Existing definitions and worked examples remain the source of truth. The 18 remaining candidates were assessed for a separate PM task, not accepted merely to reach a count.

| Category | Already covered by the 38-resource library | Distinct gap addressed in batch two |
|---|---|---|
| APIs | Terminology, methods/status codes, sync, imports, idempotency and retries. | Contract migration, interface comparison and evidence-based discovery. |
| Software Architecture | Components, deployment boundaries, async jobs, queues, caching and technical debt. | Workload planning, data identity/history and dependency failure journeys. |
| SQL & Data | Core syntax, joins, grouping, CTEs and an ordered funnel. | Time boundaries, anomaly diagnosis and executable mature-cohort retention. |
| Product Metrics | Measurement contracts, North Star/guardrails, activation, retention and churn. | Funnel interpretation, unique active audiences and mix-shift diagnosis. |
| Observability | Signal types, identifiers, percentiles and incident response. | Reproducible Kibana searches and signals tied to response ownership. |
| Security | Identity/permissions, sessions, integration access and MFA recovery. | Federated employee lifecycle and proportionate audit evidence. |
| CI/CD & Releases | Delivery terms, recovery, tests, progressive rollout and mobile releases. | Confidence across environments and readiness across dependencies. |

## Current resource counts

| Category | Before batch two | Added | Current coverage |
|---|---:|---:|---:|
| APIs | 7 | 3 | 10 |
| Software Architecture | 6 | 3 | 9 |
| SQL & Data | 5 | 3 | 8 |
| Product Metrics | 5 | 3 | 8 |
| Observability | 5 | 2 | 7 |
| Security | 5 | 2 | 7 |
| CI/CD & Releases | 5 | 2 | 7 |
| **Total** | **38** | **18** | **56** |

## Implemented candidates

These topics now belong to existing coverage. Their boundaries explain why they have standalone pages.

| Resource | Format | Distinct PM question or task |
|---|---|---|
| [API Versioning and Deprecation for Product Managers](APIs/api-versioning-and-deprecation-for-product-managers.md) | Practical guide | Plan a consumer migration with compatibility evidence and a retirement decision. |
| [REST vs GraphQL for Product Managers](APIs/rest-vs-graphql-for-product-managers.md) | Explainer | Compare interfaces against a real screen, authorization and operating constraints. |
| [API Integration Discovery Checklist for Product Managers](APIs/api-integration-discovery-checklist.md) | Reference | Turn provider unknowns into a scoped decision with evidence and owners. |
| [Scalability for Product Managers](Software-Architecture/scalability-for-product-managers.md) | Practical guide | Translate a campaign forecast into workload assumptions and capacity evidence. |
| [Databases and Product Data Models for PMs](Software-Architecture/databases-and-product-data-models-for-pms.md) | Explainer | Specify entity identity, relationships and historical truth before choosing storage. |
| [Dependencies and Graceful Degradation for PMs](Software-Architecture/dependencies-and-graceful-degradation-for-pms.md) | Practical guide | Define a truthful usable journey when a dependency fails and when it recovers. |
| [Date and Time Queries for Product Managers](SQL/date-and-time-queries-for-product-managers.md) | Cheat sheet | Select a business day across daylight-saving boundaries without losing zero-activity days. |
| [Finding Duplicates and Handling NULLs for PMs](SQL/finding-duplicates-and-handling-nulls-for-pms.md) | Practical guide | Separate repeated delivery, conflicting records and missing identity before counting. |
| [Retention and Cohort Queries for Product Managers](SQL/retention-and-cohort-queries-for-product-managers.md) | Practical guide | Calculate mature calendar-week retention with deduplicated returners and fixed cohorts. |
| [Funnel Analysis for Product Managers](Product-Metrics/funnel-analysis-for-product-managers.md) | Practical guide | Interpret drop-offs and choose an investigation without confusing friction with tracking gaps. |
| [DAU, WAU and MAU for Product Managers](Product-Metrics/dau-wau-mau-for-product-managers.md) | Cheat sheet | Define meaningful activity and distinguish unique audiences from repeated use. |
| [Evaluating a Metric Change for Product Managers](Product-Metrics/evaluating-a-metric-change-for-product-managers.md) | Practical guide | Check measurement, population mix and causal claims before reacting to a change. |
| [How to Investigate Logs in Kibana as a Product Manager](Observability/how-to-investigate-logs-in-kibana-as-a-pm.md) | Practical guide | Build and hand off a reproducible search with explicit scope and field assumptions. |
| [Actionable Dashboards and Alerts for PMs](Observability/actionable-dashboards-and-alerts-for-pms.md) | Practical guide | Connect a signal to a customer outcome, response owner and useful next action. |
| [SSO and Employee Lifecycle for Product Managers](Security/sso-and-employee-lifecycle-for-product-managers.md) | Practical guide | Separate sign-in federation from provisioning, role changes and deprovisioning. |
| [Data Minimization and Audit Logs for PMs](Security/data-minimization-and-audit-logs-for-pms.md) | Practical guide | Design useful accountability evidence without collecting unnecessary sensitive data. |
| [Environment Parity for Product Managers](CI-CD/environment-parity-for-product-managers.md) | Practical guide | Explain what staging evidence transfers to production and which differences remain. |
| [Release Dependencies and Readiness for PMs](CI-CD/release-dependencies-and-readiness-for-pms.md) | Reference | Coordinate compatible components, rollout gates and recovery ownership. |

## Topics combined or rejected as separate pages

- **Engagement and DAU/WAU/MAU** remain one page: the useful task is defining meaningful unique activity and cadence, not another generic engagement definition.
- **Duplicates and NULLs** share an input-diagnosis guide. Separate short syntax primers would repeat the SQL cheat sheet; the new page distinguishes redelivery, conflicting IDs and unknown identity.
- **Dashboards and alerts** share a decision-and-response guide. Another SLI/SLO/SLA glossary would repeat existing coverage.
- **Data minimization and audit logs** share an event-design workflow: choose useful evidence, access and retention without unnecessary sensitive payloads. A general security vocabulary page would duplicate the glossary.
- **Kibana** earns a page through reproducible search scope, KQL and field assumptions. A click-by-click interface tour is deliberately omitted because deployment/version differences would make it fragile.
- **Funnel analysis** interprets observed drop-offs; the existing SQL funnel page implements a query. **SQL retention** implements a mature population; the existing retention/cohort page defines and interprets the metric. Neither new page repeats the existing one wholesale.
- **SSO** focuses on provisioning, role changes and departure, while the existing authentication, session and MFA pages retain their explanations.
- Standalone introductions to retries, webhooks, cache, roles, feature flags, test layers and rollback would repeat current content and were not added.
- Scalability does not introduce a preferred architecture ladder. Readiness coordinates dependencies; environment parity qualifies test evidence rather than repeating the CI/CD glossary.

## Remaining editorial possibilities

The earlier candidate list is now implemented with the boundaries above. The re-audit surfaced narrower possibilities that should be validated against reader questions before drafting:

| Possible topic | Distinct need to validate | Boundary against existing coverage |
|---|---|---|
| Stored-data migrations and backfills | Plan reconciliation and recovery when historical data must change | Go beyond API versioning and release readiness without becoming a migration-engineering tutorial |
| Instrumentation changes and metric continuity | Preserve interpretation when event schemas or tracking change | Focus on change control and reconciliation, not another general metric-change checklist |
| Machine identity ownership | Transfer or retire service access independently of employee accounts | Extend the SSO lifecycle gap without repeating API keys/OAuth |
| Retention and deletion propagation | Trace a reviewed data policy through derived copies and exports | Extend minimization with lifecycle evidence; avoid prescriptive legal rules |

These are editorial possibilities, not scheduled resources. Combine them with existing pages if that serves readers better; discard them if the distinct need is not established.

## Future category ideas

AI & LLMs, Cloud & Infrastructure, Experimentation, Privacy and other categories remain outside the current expansion. Validate demand and overlap first. Data, mobile delivery, integration and performance topics already have homes in the existing seven categories; a new heading is not automatically necessary.

## Review criteria

A resource should answer a useful PM question independently, preserve technical qualifications, link where the next question naturally arises, and provide exactly one relevant final book CTA. SQL needs executable synthetic examples and honest test claims. Reader value and accuracy determine additions, not a numerical publishing target.
