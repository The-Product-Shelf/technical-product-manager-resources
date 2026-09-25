# Technical Product Manager Resources

**Technical skills for Product Managers, explained simply.**

**Free practical resources for Product Managers who want to become more technical — without becoming developers.**

Created by **The Product Shelf**.

**You don't need to become an engineer to become a more technical Product Manager.**

**56 resources · 7 topics · No coding required**

Product Managers work with APIs, databases, architecture, logs, deployments and security every day. You don't need to know how to build these systems. But understanding how they work helps you **ask better questions, have better conversations with engineers and make better product decisions.**

This library contains 56 practical guides, cheat sheets and glossaries. Each resource stands on its own, with plain-English explanations, illustrative product scenarios and questions to bring to your team. Start with the topic closest to your current work.

## Not sure where to start?

Follow this **suggested learning path** if you would like a sequence that connects the topics:

APIs → Software Architecture → SQL & Data → Product Metrics → Observability → Security → CI/CD & Releases

This is a suggestion, not a required order. Each resource stands on its own, so jump straight to the question you are working on. The SQL examples introduce the syntax as you go; no prior coding knowledge is needed.

## 📡 APIs

- [API Terminology for Product Managers](APIs/api-terminology-for-product-managers.md) — Understand API contracts, requests and integration constraints.
- [HTTP Methods for Product Managers](APIs/http-methods-for-product-managers.md) — Choose the operation and understand safe versus idempotent behavior.
- [HTTP Status Codes for Product Managers](APIs/http-status-codes-for-product-managers.md) — Interpret responses and plan recovery without confusing partial failures with failed user tasks.
- [Polling vs Webhooks for Product Managers](APIs/polling-vs-webhooks-for-product-managers.md) — Define freshness, delivery failure and reconciliation for calendar sync.
- [Pagination and Rate Limits for Product Managers](APIs/pagination-and-rate-limits-for-product-managers.md) — Plan a complete import under changing data and request limits.
- [Idempotency for Product Managers](APIs/idempotency-for-product-managers.md) — Prevent duplicate business operations when a request is repeated.
- [API Timeouts and Retries for Product Managers](APIs/api-timeouts-and-retries-for-product-managers.md) — Design pending states, retry limits and recovery for uncertain outcomes.
- [API Versioning and Deprecation for Product Managers](APIs/api-versioning-and-deprecation-for-product-managers.md) — Plan a consumer migration with compatibility evidence and a retirement decision.
- [REST vs GraphQL for Product Managers](APIs/rest-vs-graphql-for-product-managers.md) — Compare interfaces against a real screen, authorization and operating constraints.
- [API Integration Discovery Checklist for Product Managers](APIs/api-integration-discovery-checklist.md) — Turn provider unknowns into a scoped decision with evidence and owners.

## 🏗 Software Architecture

- [Software Architecture Glossary for PMs](Software-Architecture/software-architecture-glossary-for-pms.md) — Connect components, dependencies and performance to the user experience.
- [Monolith vs Microservices for PMs](Software-Architecture/monolith-vs-microservices-for-pms.md) — Evaluate delivery and operational trade-offs behind an architecture proposal.
- [Synchronous vs Asynchronous Processing for PMs](Software-Architecture/synchronous-vs-asynchronous-processing-for-pms.md) — Separate acceptance from completion and specify the job lifecycle.
- [Queues and Message Brokers for PMs](Software-Architecture/queues-and-message-brokers-for-pms.md) — Reason about backlogs, redelivery, ordering and failed work.
- [Caching for Product Managers](Software-Architecture/caching-for-product-managers.md) — Agree on acceptable freshness for different product data.
- [Technical Debt for Product Managers](Software-Architecture/technical-debt-for-product-managers.md) — Turn a debt proposal into evidence, options and a measurable outcome.
- [Scalability for Product Managers](Software-Architecture/scalability-for-product-managers.md) — Translate a campaign forecast into workload assumptions and capacity evidence.
- [Databases and Product Data Models for PMs](Software-Architecture/databases-and-product-data-models-for-pms.md) — Specify entity identity, relationships and historical truth before choosing storage.
- [Dependencies and Graceful Degradation for PMs](Software-Architecture/dependencies-and-graceful-degradation-for-pms.md) — Define a truthful usable journey when a dependency fails and when it recovers.

## 🗄 SQL & Data

- [SQL Cheat Sheet for Product Managers](SQL/sql-cheat-sheet-for-product-managers.md) — Filter and summarize activity while checking what the result actually measures.
- [SQL JOINs Explained for Product Managers](SQL/sql-joins-explained-for-product-managers.md) — Combine tables without losing users or multiplying counts unexpectedly.
- [GROUP BY, WHERE and HAVING for Product Managers](SQL/group-by-where-and-having-for-product-managers.md) — Build segment summaries and distinguish input filters from group filters.
- [CTEs for Product Managers](SQL/ctes-for-product-managers.md) — Make a multi-stage analysis readable and check its intermediate populations.
- [Product Funnel Queries for Product Managers](SQL/product-funnel-queries-for-product-managers.md) — Implement an ordered, bounded funnel with reproducible sample data.
- [Date and Time Queries for Product Managers](SQL/date-and-time-queries-for-product-managers.md) — Select a business day across daylight-saving boundaries without losing zero-activity days.
- [Finding Duplicates and Handling NULLs for PMs](SQL/finding-duplicates-and-handling-nulls-for-pms.md) — Separate repeated delivery, conflicting records and missing identity before counting.
- [Retention and Cohort Queries for Product Managers](SQL/retention-and-cohort-queries-for-product-managers.md) — Calculate mature calendar-week retention with deduplicated returners and fixed cohorts.

## 📊 Product Metrics

- [Product Metrics Cheat Sheet](Product-Metrics/product-metrics-cheat-sheet.md) — Define populations, time windows and calculations before comparing numbers.
- [North Star, Input Metrics & Guardrails](Product-Metrics/north-star-input-and-guardrail-metrics.md) — Connect customer value, possible drivers and the side effects you need to monitor.
- [Activation for Product Managers](Product-Metrics/activation-for-product-managers.md) — Select and validate a first-value hypothesis and measurement window.
- [Retention and Cohorts for Product Managers](Product-Metrics/retention-and-cohorts-for-product-managers.md) — Read a cohort table with consistent eligibility and observation time.
- [Churn for Product Managers](Product-Metrics/churn-for-product-managers.md) — Distinguish customer loss, revenue loss and expansion with a worked ledger.
- [Funnel Analysis for Product Managers](Product-Metrics/funnel-analysis-for-product-managers.md) — Interpret drop-offs and choose an investigation without confusing friction with tracking gaps.
- [DAU, WAU and MAU for Product Managers](Product-Metrics/dau-wau-mau-for-product-managers.md) — Define meaningful activity and distinguish unique audiences from repeated use.
- [Evaluating a Metric Change for Product Managers](Product-Metrics/evaluating-a-metric-change-for-product-managers.md) — Check measurement, population mix and causal claims before reacting to a change.

## 🔎 Observability

- [Observability Glossary for Product Managers](Observability/observability-glossary-for-product-managers.md) — Understand telemetry, reliability targets and the limits of the evidence.
- [Logs vs Metrics vs Traces](Observability/logs-vs-metrics-vs-traces.md) — Investigate a reported failure and hand over useful context to Engineering.
- [Reading Logs and Correlation IDs for PMs](Observability/reading-logs-and-correlation-ids-for-pms.md) — Build a timeline across retries without treating IDs or log levels as proof.
- [Percentiles, p95 and p99 for PMs](Observability/percentiles-p95-and-p99-for-pms.md) — Interpret tail latency, measurement windows and segment effects.
- [Production Incident Investigation for PMs](Observability/production-incident-investigation-for-pms.md) — Structure evidence, impact, escalation and recovery checks.
- [How to Investigate Logs in Kibana as a Product Manager](Observability/how-to-investigate-logs-in-kibana-as-a-pm.md) — Build and hand off a reproducible search with explicit scope and field assumptions.
- [Actionable Dashboards and Alerts for PMs](Observability/actionable-dashboards-and-alerts-for-pms.md) — Connect a signal to a customer outcome, response owner and useful next action.

## 🔐 Security

- [Security Glossary for Product Managers](Security/security-glossary-for-product-managers.md) — Discuss access, data protection and risk in everyday product decisions.
- [Authentication vs Authorization](Security/authentication-vs-authorization.md) — Define who can act on which resources, including when access changes.
- [Sessions for Product Managers](Security/sessions-for-product-managers.md) — Specify expiry, logout and revocation across devices and organizations.
- [API Keys vs OAuth for Product Managers](Security/api-keys-vs-oauth-for-product-managers.md) — Choose an access model for service and user-delegated integrations.
- [MFA and Account Recovery for Product Managers](Security/mfa-and-account-recovery-for-product-managers.md) — Protect sensitive actions while designing usable recovery paths.
- [SSO and Employee Lifecycle for Product Managers](Security/sso-and-employee-lifecycle-for-product-managers.md) — Separate sign-in federation from provisioning, role changes and deprovisioning.
- [Data Minimization and Audit Logs for PMs](Security/data-minimization-and-audit-logs-for-pms.md) — Design useful accountability evidence without collecting unnecessary sensitive data.

## 🚀 CI/CD & Releases

- [CI/CD Glossary for Product Managers](CI-CD/cicd-glossary-for-product-managers.md) — Follow a change from code through checks and environments.
- [Deployment vs Release vs Rollback vs Feature Flag](CI-CD/deployment-release-rollback-feature-flags.md) — Plan exposure, stopping conditions and recovery.
- [Automated Tests for Product Managers](CI-CD/automated-tests-for-product-managers.md) — Connect test layers to product risks and remaining uncertainty.
- [Progressive Rollouts for Product Managers](CI-CD/progressive-rollouts-for-product-managers.md) — Define exposure units, evidence gates and stop conditions.
- [Mobile App Releases for Product Managers](CI-CD/mobile-app-releases-for-product-managers.md) — Coordinate store distribution, installed versions and backend compatibility.
- [Environment Parity for Product Managers](CI-CD/environment-parity-for-product-managers.md) — Explain what staging evidence transfers to production and which differences remain.
- [Release Dependencies and Readiness for PMs](CI-CD/release-dependencies-and-readiness-for-pms.md) — Coordinate compatible components, rollout gates and recovery ownership.

## Using the resources

The fictional products and sample data make each concept concrete. Example metrics, policies and launch conditions are discussion aids, not universal benchmarks. Adapt them with Engineering, Data and Security to your product's actual behavior.

Found a mistake or an unclear explanation? Corrections and suggestions are welcome through [Issues](https://github.com/The-Product-Shelf/technical-product-manager-resources/issues) or pull requests. See [how to contribute](CONTRIBUTING.md). Please keep customer information and credentials out of public examples.

## License

The original educational content in this repository is licensed under **[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/)**. You may share and adapt it for noncommercial purposes, subject to the license terms, including crediting The Product Shelf, linking to the license and indicating changes.

This license does not cover the books or other materials linked from these resources. See [LICENSE.md](LICENSE.md) for scope, attribution guidance and the full license terms.

## About The Product Shelf

**The Product Shelf** is a collection of practical books and learning resources designed to help Product Managers understand the technical side of building software.

→ [Explore The Product Shelf](https://theproductshelf.com/)
