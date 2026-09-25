# Dependencies and Graceful Degradation for PMs

## Decide what remains usable during a failure

Nómada depends on payment, address validation and product recommendations. If recommendations fail, checkout might still work. If payment outcome is unknown, claiming the order failed and inviting another payment can be harmful. Dependencies need different fallback policies based on the customer promise.

**Graceful degradation** means providing a deliberately limited, truthful experience when part of the system is unavailable. It is not a synonym for silently ignoring errors or treating stale information as current.

## Map a journey's dependencies

| Dependency | Failure consequence | Candidate behavior to review |
|---|---|---|
| Recommendations | Suggestions are unavailable | Omit the section while preserving browsing |
| Address validation | Deliverability cannot be confirmed automatically | Save a draft or offer reviewed manual handling if the business supports it |
| Payment provider | Charge outcome may be uncertain | Preserve the order reference and resolve its state before repeating payment |
| Shipment tracking | Latest delivery status cannot be fetched | Show the last known update with its timestamp |
| Identity/permission service | Access cannot be established reliably | Follow the reviewed access policy; do not assume permission |

These are discussion examples, not universal defaults. A dependency optional for one journey can be essential for another. The same unavailable recommendation service could block a product whose central feature is generating recommendations.

## Separate containment from fallback

A timeout limits waiting. A circuit breaker can stop sending repeated calls to a dependency judged unhealthy and later allow recovery probes. Isolation can keep one overloaded part from exhausting resources used elsewhere. Engineering chooses mechanisms; Product specifies the supported experience when they take effect.

```text
Dependency unavailable
  -> detect and contain failure
  -> provide an approved limited experience, or stop clearly
  -> recover and reconcile unfinished work
```

A circuit breaker does not create a fallback result, reverse a payment or prove that all earlier requests failed. Its state is operational evidence, not the customer's order state. See [timeouts and retries](../APIs/api-timeouts-and-retries-for-product-managers.md).

## Worked scenario: tracking comes back

During an outage, Nómada shows “Last update: dispatched at 09:10” instead of “Arriving now.” When tracking returns, the product needs to refresh the status without sending every missed notification as if it were new. If a delivery occurred during the outage, the customer should receive the appropriate current outcome rather than a confusing sequence of stale messages.

Agree the source of truth, refresh trigger, backlog owner and duplicate-notification behavior. Recovery is part of the feature; hiding the error message is not enough. If the fallback uses cached data, define freshness and which actions are disabled while it is stale.

## Exercise the failure before launch

Ask Engineering to demonstrate a slow dependency, a hard failure and recovery under an approved test plan. Observe customer messaging, pending work and the signals Support can use. Include shared dependencies: a small new feature can affect unrelated journeys when it consumes shared capacity.

Who can enable a degraded mode? Which promises are suspended? What requires customer follow-up? When does limited operation become unacceptable? Record these answers with the release plan.

Related: [caching](caching-for-product-managers.md), [asynchronous work](synchronous-vs-asynchronous-processing-for-pms.md) and [incident investigation](../Observability/production-incident-investigation-for-pms.md).

## Primary reference

[Microsoft's Circuit Breaker pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/circuit-breaker) explains fault containment and recovery considerations. It is one mechanism, not a complete customer-experience policy.

---

### Design the product around dependency limits

**Software Architecture for Product Managers** — explore Nómada’s architecture and the trade-offs between availability, consistency and recovery.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
