# API Timeouts and Retries for Product Managers

What should the user see when a dependency takes too long, and when should the system try again?

Nubira is a fictional booking platform. After saving an appointment, it asks a calendar provider to create an event. A slow or missing response introduces two different questions: how long should the caller wait, and what actually happened to the requested operation?

## A timeout is a waiting limit

A timeout means a component stopped waiting under its configured rules. It does not prove that the remote operation stopped or never happened. The client may receive no HTTP response at all; a 504 is one particular gateway response, not the name for every timeout.

| Observed situation | What remains uncertain | Product response to define |
| --- | --- | --- |
| Request rejected with a documented validation error | Which input or rule must change? | Explain the recoverable problem rather than endlessly retrying. |
| Provider indicates temporary unavailability | When will it recover, and is repeating this operation safe? | Preserve progress and choose a bounded retry strategy. |
| Request times out after submission | Did the provider complete it? | Check the outcome or use a documented safe retry mechanism. |
| Processing was accepted for later completion | When and how will the final result arrive? | Show pending status and track completion separately. |

Use the [status-code guide](http-status-codes-for-product-managers.md) for response meanings. [HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.2) explains why retrying a non-idempotent operation requires care.

## Separate user patience from background recovery

Suppose the appointment is saved in Nubira, but calendar synchronization is delayed. The user need not remain on a spinner indefinitely. A truthful experience can confirm the booking and show that calendar synchronization is pending, provided the system tracks and follows up on that work.

Agree on a visible waiting threshold, a background retry deadline and a terminal outcome such as “needs attention.” These are different limits. A process that retries forever may hide work that will never complete and give Support no useful state to investigate.

For [asynchronous work](../Software-Architecture/synchronous-vs-asynchronous-processing-for-pms.md), define how the user discovers the final result after leaving the screen.

## A retry policy needs more than a number

**Backoff** increases the interval between attempts. **Jitter** varies timing so many clients do not all retry together. A bounded retry budget limits the extra work caused by failures. Engineering should choose the mechanism based on the dependency and workload.

Imagine several services each retrying three times. One user action can produce many more downstream attempts when retries compound across layers. That extra load can make an overloaded dependency less likely to recover. Ask which layer owns retrying and whether all layers share an overall deadline.

Provider instructions such as `Retry-After` matter, but waiting alone does not make a write safe. Repeated creation requests also need a supported [idempotency mechanism](idempotency-for-product-managers.md).

## Review the recovery experience

For Nubira, walk through a failure that lasts longer than the automatic retry window. Can the clinic identify appointments that are not synced? Can it retry selected failures without duplicating successful events? What happens if access is revoked instead of the provider being unavailable?

A useful acceptance discussion covers the displayed state, retry ownership, duplicate protection, escalation route and eventual reconciliation. Avoid displaying “Try again” before the team has explained what that action will repeat.

For a concrete design reference, Microsoft's [Retry pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/retry) discusses transient failures and retry considerations. For scheduled imports, see [pagination and rate limits](pagination-and-rate-limits-for-product-managers.md).

---

### Design the experience around API uncertainty

**APIs for Product Managers** — follow Nubira’s integrations and learn to ask what succeeded, what remains pending and what the user can safely do next.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
