# API Versioning and Deprecation for Product Managers

## Plan a migration, not just a new URL

Nubira's calendar provider is retiring an older appointment API. The clinic does not care which version number is in the request; it cares whether existing appointments remain synchronized. Product needs a migration plan that covers active consumers, behavior changes and evidence of completion.

**Versioning** distinguishes API contracts. **Deprecation** signals that a capability should no longer be relied on for future use under the provider's policy. **Retirement** removes or ends support for it. Deprecation does not necessarily mean immediate failure, and a version label does not specify a support lifetime.

## Inventory the actual consumers

| Record | Example for Nubira | Decision it supports |
|---|---|---|
| Consumer and owner | Calendar worker; integrations team | Who can ship and verify the change? |
| Selected contract | Header, path or provider default | Are we pinning a version or inheriting a moving default? |
| Used operations | Create, reschedule, cancel and reconcile | Which workflows must be checked? |
| Changed behavior | Cancellation now needs an explicit reason | Is a UI or data change required? |
| Deadline and policy | Provider's documented retirement date | What is the latest safe migration window? |
| Completion evidence | Successful real workflows and no old-version traffic | What supports retirement of the old path? |

Include infrequent jobs and customer-managed integrations. A weekly reconciliation task will not appear in a ten-minute traffic sample. An integration with no current owner is an unresolved dependency, not evidence it is unused.

## A compatible shape can hide an incompatible meaning

Removing a field or requiring a new parameter is an obvious risk. Changes to defaults, ordering, permissions, pagination or event timing can also change the experience. An added enum value may break a consumer that assumes its list is exhaustive, even if the provider classifies the addition as non-breaking.

Compare representative booking journeys, not only response schemas. Nubira should check whether a cancellation both changes the source record and eventually updates the clinic's calendar. A successful request alone does not verify the downstream outcome.

## Make the transition reversible where possible

```text
Inventory consumers -> Compare contracts -> Adapt and test
  -> Migrate a controlled audience -> Observe complete workflows
  -> Retire the old path when evidence and policy allow
```

Running both versions for comparison must not duplicate real bookings or notifications. Engineering should choose a safe comparison strategy. Switching back may be impossible after data or provider behavior changes; identify those limits before scheduling the cutover.

Provide customers with the action required, affected workflows, dates and support route. Keep an owner for consumers that cannot migrate on time. A sent announcement is communication evidence, not migration evidence.

## Questions for a migration review

What continues working during the support window? Are webhooks versioned separately? How do we recognize traffic from the old contract? Does rollback restore behavior or only request formatting? What happens to jobs accepted before the switch?

[Integration discovery](api-integration-discovery-checklist.md) helps collect evidence; [automated tests](../CI-CD/automated-tests-for-product-managers.md) helps frame compatibility checks. Preserve [idempotency](idempotency-for-product-managers.md) when requests cross a migration boundary.

## Primary references

[GitHub's versioning documentation](https://docs.github.com/en/rest/about-the-rest-api/api-versions) is a concrete provider policy, not a universal guarantee. [RFC 8594](https://www.rfc-editor.org/rfc/rfc8594.html) defines the Sunset response header for communicating likely future unavailability; check whether the provider actually uses it.

---

### Understand the contract through change

**APIs for Product Managers** — follow Nubira’s API scenarios to connect documentation, integration behavior and customer expectations.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
