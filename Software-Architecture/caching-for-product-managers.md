# Caching for Product Managers

How fresh does information need to be, and where is temporarily stale data acceptable?

Nómada is a fictional online shop. Its catalog is slow, and Engineering suggests caching product information. The opportunity is faster access to a stored copy. The product decision is which information can tolerate that copy being behind the authoritative source.

## Quick Reference

| Term | Plain-English meaning | Product implication |
| --- | --- | --- |
| Cache hit | The requested copy is available. | A fast response can still contain old information. |
| Cache miss | The copy is absent or unusable. | The system may need slower work to obtain the value. |
| TTL | A time-to-live used to govern expiry. | It is not automatically a guarantee about every user's observed freshness. |
| Invalidation | Making a cached value unusable after change. | Ask how quickly a correction reaches all relevant copies. |
| Cache key | What distinguishes one cached result from another. | Context such as customer, currency or permissions may matter. |
| Eviction | Removing a value, often to make room. | Cache contents should not be the sole durable record of essential data. |

Caching can exist in browsers, CDNs, application services and databases. Clearing one layer may not update another. Microsoft's [caching guidance](https://learn.microsoft.com/en-us/azure/architecture/best-practices/caching) discusses freshness, expiry and invalidation trade-offs.

## Agree on a freshness policy per use case

The following are discussion prompts for Nómada, not recommended expiry times.

| Information | What stale data could cause | Requirement to discuss |
| --- | --- | --- |
| Product image | The customer sees an older photo. | How are important image corrections propagated? |
| Display price | The checkout differs from the listing. | Where is the final price validated and explained? |
| Stock indication | An item appears available after selling out. | What happens when availability changes before purchase? |
| Order status | A customer thinks a shipped order is still pending. | How is freshness communicated when it affects an action? |
| Access permissions | A removed user still appears entitled. | How are revocations enforced independently of stale UI state? |

“Use a five-minute cache everywhere” ignores these differences. A short expiry alone also does not guarantee immediate revocation or a consistent checkout.

## Example: a price changes mid-purchase

Laura views a backpack at one price. Before she checks out, an administrator corrects that price. Product needs a policy: honor the displayed price under specified conditions, require confirmation of the new total, or another agreed behavior.

Engineering then needs to know which record establishes the offer, how long it remains valid and where the checkout validates it. The cache is one part of the mechanism; it cannot choose the commercial policy.

Test the transition, not only the steady state. Updating a product should be followed by checks of the listing, detail page, cart and checkout. These may not all use the same copy.

## Faster does not mean failure-proof

A cache can reduce load on a dependency. When many popular entries expire together, requests may suddenly reach that dependency. When it is unavailable, serving an older copy may be possible for some data but inappropriate for other data.

Ask which stale fallback is intentional, what maximum age it permits and how the user or operator knows it is happening. Do not generalize a safe catalog-image fallback to permissions or financial balances.

For diagnosis, compare latency and freshness by route and audience; a high overall hit rate can hide a slow critical path.

Related: the [architecture glossary](software-architecture-glossary-for-pms.md) introduces components, while [p95 and p99](../Observability/percentiles-p95-and-p99-for-pms.md) helps assess the response-time distribution.

## Related resources

- [Dependencies and Graceful Degradation for PMs](dependencies-and-graceful-degradation-for-pms.md) — Define a truthful usable journey when a dependency fails and when it recovers.

---

### Evaluate speed and freshness together

**Software Architecture for Product Managers** — follow Nómada as performance improvements introduce decisions about data, dependencies and the customer experience.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
