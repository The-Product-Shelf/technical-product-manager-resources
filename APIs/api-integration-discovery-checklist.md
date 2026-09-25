# API Integration Discovery Checklist for Product Managers

## End discovery with a decision and its evidence

A provider saying “we have an API” does not establish that Nubira can keep a clinic's calendars synchronized. Use this checklist before committing the integration's scope. Its output is a short evidence record, not a list of terminology to memorize.

Write the proposed promise first: “Appointments created, changed or canceled in Nubira will be reflected in the connected calendar, with visible recovery when synchronization is delayed.” Then test whether the provider supports every part.

## Evidence checklist

| Area | Evidence to obtain | Decision if evidence is missing |
|---|---|---|
| Required operations | Documented create, update and cancel behavior plus a representative test | Reduce scope or investigate before promising two-way sync |
| Permissions and ownership | Which actor grants access, permitted calendars, revocation behavior | Define admin setup and reconnect ownership |
| Change discovery | Events or queries that reveal edits and deletions | Agree freshness and reconciliation limits |
| Initial import | Pagination, history coverage and quota behavior | Set a realistic import experience |
| Failure recovery | Outcome lookup, duplicate protection and retry rules | Design pending states before adding retry actions |
| Lifecycle | Supported versions, retirement policy and change notices | Assign ongoing maintenance responsibility |
| Data handling | Fields exchanged, retention and approved logging | Review unnecessary collection with Security |
| Operational support | Status visibility, support route and diagnostic references | Define who owns customer incidents |

Record the source, date checked, owner and status for each claim. Useful statuses are **verified**, **documented but untested**, **unknown** and **unsupported**. A blank cell should not be treated as a yes.

## Worked decision: a narrower launch

Suppose creating appointments works in a sandbox, but the provider offers no documented deletion feed. Product has three different choices: investigate an alternative reconciliation mechanism, launch one-way creation with an explicit limit, or defer the integration. “Full sync, subject to API limitations” hides the decision from customers.

A useful entry would read:

```text
Claim: cancellations made in the external calendar appear in Nubira.
Evidence: no deletion event documented; list API behavior not verified.
Status: unknown.
Owner: integrations engineer.
Next check: test whether removed records can be detected reliably.
Scope consequence: external cancellation sync is not launch-ready.
```

The entry is fictional. Do not place real credentials or customer records in public discovery notes.

## Choose tests that challenge the promise

Walk through a successful operation, revoked permission, an uncertain timeout and a reconnect after missed changes. Use synthetic appointments and approved test accounts. A sandbox may differ from production in quotas, review requirements or available events; record those gaps rather than presenting sandbox success as complete validation.

An OpenAPI description can establish documented request and response shapes, but it does not prove operational reliability or cover every webhook and business guarantee. The [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) defines that description format.

## Close the open questions explicitly

Before estimating delivery, identify which unknowns could change architecture or customer promises. Assign a bounded investigation and a decision date. Carry any accepted limitation into requirements, customer wording and support guidance. Revisit evidence when the provider changes its contract.

For detailed mechanics, use [polling vs webhooks](polling-vs-webhooks-for-product-managers.md), [timeouts and retries](api-timeouts-and-retries-for-product-managers.md), [pagination](pagination-and-rate-limits-for-product-managers.md) and [versioning](api-versioning-and-deprecation-for-product-managers.md). This checklist coordinates those questions; it does not replace their answers.

---

### Scope an integration with better evidence

**APIs for Product Managers** — follow Nubira through API documentation and product requirements, including the limitations behind a working connection.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
