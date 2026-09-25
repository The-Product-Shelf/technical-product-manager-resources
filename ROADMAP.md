# Library Expansion Roadmap

Technical skills for Product Managers, explained simply.

This roadmap was prepared from the published 14-resource library before drafting the expansion. It separates the current batch from candidates that require further validation. It is a **candidate coverage map for possible future expansion** within the existing seven categories. Candidates are editorial possibilities, not a quota or a publishing commitment.

## Audit of the starting library

| Category | Covered sufficiently in the existing resources | Gap selected for this batch |
| --- | --- | --- |
| APIs | Terminology, HTTP response interpretation and partial-failure example. | Request behavior, synchronization, imports and safe recovery. |
| Software Architecture | Component vocabulary, monolith/microservice trade-offs and basic asynchronous/caching examples. | Lifecycle design, messaging behavior, freshness policy and debt prioritization. |
| SQL & Data | Core syntax, counts, date boundaries, INNER/LEFT JOIN, row multiplication and current/historical attributes. | Deeper grouping decisions, staged queries and a runnable ordered funnel. |
| Product Metrics | Basic definitions, denominators, percentage-point distinction, North Star inputs and guardrails. | Operational definitions of activation, cohort retention and different forms of churn. |
| Observability | Signal comparison, identifiers, basic SLI/SLO/SLA definitions and a transaction investigation. | Correlating attempts, reading latency distributions and coordinating an incident investigation. |
| Security | Core terms, a resource-scoped permission matrix and role-change concerns. | Session lifecycle, delegated integration access and authentication recovery. |
| CI/CD & Releases | Delivery vocabulary, deployment/exposure distinction and basic recovery options. | Rollout decision gates, test evidence and mobile version coexistence. |

## Resource counts

| Category | Starting library | New in this batch | After this batch | Later candidates |
| --- | --- | --- | --- | --- |
| APIs | 2 | 5 | 7 | 3 |
| Software-Architecture | 2 | 4 | 6 | 3 |
| SQL | 2 | 3 | 5 | 3 |
| Product-Metrics | 2 | 3 | 5 | 3 |
| Observability | 2 | 3 | 5 | 2 |
| Security | 2 | 3 | 5 | 2 |
| CI-CD | 2 | 3 | 5 | 2 |
| **Total** | **14** | **24** | **38** | **18** |

## Selected batch and boundaries

The new pieces go beyond definitions already present in the library. Each has a distinct question, worked example or task. Existing explanations remain in place and receive only useful cross-links.

| Resource | Format | Additional value |
| --- | --- | --- |
| [HTTP Methods for Product Managers](APIs/http-methods-for-product-managers.md) | Cheat sheet | Choose the operation and understand safe versus idempotent behavior. |
| [Polling vs Webhooks for Product Managers](APIs/polling-vs-webhooks-for-product-managers.md) | Explainer | Define freshness, delivery failure and reconciliation for calendar sync. |
| [Pagination and Rate Limits for Product Managers](APIs/pagination-and-rate-limits-for-product-managers.md) | Practical guide | Plan a complete import under changing data and request limits. |
| [Idempotency for Product Managers](APIs/idempotency-for-product-managers.md) | Explainer | Prevent duplicate business operations when a request is repeated. |
| [API Timeouts and Retries for Product Managers](APIs/api-timeouts-and-retries-for-product-managers.md) | Practical guide | Design pending states, retry limits and recovery for uncertain outcomes. |
| [Synchronous vs Asynchronous Processing for PMs](Software-Architecture/synchronous-vs-asynchronous-processing-for-pms.md) | Explainer | Separate acceptance from completion and specify the job lifecycle. |
| [Queues and Message Brokers for PMs](Software-Architecture/queues-and-message-brokers-for-pms.md) | Explainer | Reason about backlogs, redelivery, ordering and failed work. |
| [Caching for Product Managers](Software-Architecture/caching-for-product-managers.md) | Practical guide | Agree on acceptable freshness for different product data. |
| [Technical Debt for Product Managers](Software-Architecture/technical-debt-for-product-managers.md) | Practical guide | Turn a debt proposal into evidence, options and a measurable outcome. |
| [GROUP BY, WHERE and HAVING for Product Managers](SQL/group-by-where-and-having-for-product-managers.md) | Cheat sheet | Build segment summaries and distinguish input filters from group filters. |
| [CTEs for Product Managers](SQL/ctes-for-product-managers.md) | Practical guide | Make a multi-stage analysis readable and check its intermediate populations. |
| [Product Funnel Queries for Product Managers](SQL/product-funnel-queries-for-product-managers.md) | Practical guide | Implement an ordered, bounded funnel with reproducible sample data. |
| [Activation for Product Managers](Product-Metrics/activation-for-product-managers.md) | Practical guide | Select and validate a first-value hypothesis and measurement window. |
| [Retention and Cohorts for Product Managers](Product-Metrics/retention-and-cohorts-for-product-managers.md) | Practical guide | Read a cohort table with consistent eligibility and observation time. |
| [Churn for Product Managers](Product-Metrics/churn-for-product-managers.md) | Cheat sheet | Distinguish customer loss, revenue loss and expansion with a worked ledger. |
| [Reading Logs and Correlation IDs for PMs](Observability/reading-logs-and-correlation-ids-for-pms.md) | Practical guide | Build a timeline across retries without treating IDs or log levels as proof. |
| [Percentiles, p95 and p99 for PMs](Observability/percentiles-p95-and-p99-for-pms.md) | Explainer | Interpret tail latency, measurement windows and segment effects. |
| [Production Incident Investigation for PMs](Observability/production-incident-investigation-for-pms.md) | Practical guide | Structure evidence, impact, escalation and recovery checks. |
| [Sessions for Product Managers](Security/sessions-for-product-managers.md) | Practical guide | Specify expiry, logout and revocation across devices and organizations. |
| [API Keys vs OAuth for Product Managers](Security/api-keys-vs-oauth-for-product-managers.md) | Explainer | Choose an access model for service and user-delegated integrations. |
| [MFA and Account Recovery for Product Managers](Security/mfa-and-account-recovery-for-product-managers.md) | Practical guide | Protect sensitive actions while designing usable recovery paths. |
| [Progressive Rollouts for Product Managers](CI-CD/progressive-rollouts-for-product-managers.md) | Practical guide | Define exposure units, evidence gates and stop conditions. |
| [Automated Tests for Product Managers](CI-CD/automated-tests-for-product-managers.md) | Reference | Connect test layers to product risks and remaining uncertainty. |
| [Mobile App Releases for Product Managers](CI-CD/mobile-app-releases-for-product-managers.md) | Practical guide | Coordinate store distribution, installed versions and backend compatibility. |

## Later candidates

These are editorial possibilities, not a publication quota. Revisit them after observing reader questions and use. Titles describe intended boundaries; no files or links are promised yet.

### APIs

- API versioning and deprecation — migration planning and consumer communication.
- REST vs GraphQL — matching client needs to a documented interface.
- API integration discovery checklist — capability validation before commitment.

### Software-Architecture

- Scalability — workload dimensions and capacity evidence.
- Databases and product data models — relationships and ownership.
- Dependencies and graceful degradation — usable journeys during provider failures.

### SQL

- Date and time queries — business days, timezones and observation windows.
- Finding duplicates and handling NULLs — diagnose data quality before aggregation.
- Retention and cohort queries — implement the metric definitions with mature cohorts.

### Product-Metrics

- Funnel analysis — interpret drop-offs, identity and ordered steps.
- Engagement and DAU/WAU/MAU — meaningful activity at the product’s natural cadence.
- Evaluating a metric change — mix shifts, tracking checks and causal uncertainty.

### Observability

- Kibana investigation walkthrough — search and filtering in a stated version.
- Actionable dashboards and alerts — signals, owners and response decisions.

### Security

- SSO and employee lifecycle — identity-provider boundaries and deprovisioning.
- Data minimization and audit logs — purpose, retention and accountability.

### CI-CD

- Environment parity — what staging can and cannot establish.
- Release dependencies and readiness — coordinate components, people and communications.

## Topics intentionally not split into new files

- Frontend/backend and client/server already have useful explanations in the architecture glossary. A second introductory definition would add little.
- WHERE vs HAVING and GROUP BY are treated together because the useful task is building a correct grouped analysis.
- Webhooks and polling share one comparison; pagination and rate limits share an import-planning guide. Separate short definitions would duplicate the API glossary.
- A new authentication-versus-authorization or roles-and-permissions primer would duplicate the existing policy example. The access-model guide focuses on integration credentials instead.
- A separate feature-flags glossary, hotfix definition or rollback-versus-roll-forward primer would repeat the release guide. The rollout guide adds audience selection and evidence gates.
- A separate SLI/SLO/SLA glossary, guardrail primer or percentage-points explainer is not needed in this batch. Their core distinctions are already covered.
- Retention/cohorts is a metric-design guide; the later SQL cohort candidate would implement those definitions. The current SQL funnel guide concentrates on query correctness rather than a second glossary of conversion metrics.

## Future category ideas

AI & LLMs, Data & Analytics, Cloud & Infrastructure, Mobile Apps, Integrations, Experimentation, Performance and Privacy may warrant separate categories later. No new category is introduced here. First assess reader demand and overlap with the existing seven categories.

## Review criteria

A resource earns its place by answering a useful PM question independently, retaining technical qualifications, linking to related material and providing one relevant final book link. Scope, accuracy and reader needs determine which candidates, if any, become resources.
