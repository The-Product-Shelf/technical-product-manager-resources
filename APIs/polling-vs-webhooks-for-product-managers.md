# Polling vs Webhooks for Product Managers

How will your product learn that something changed in another system?

Nubira is a fictional booking platform. A clinic edits an appointment in its external calendar and expects Nubira to reflect the change. The choice between checking periodically and receiving notifications affects freshness, operating cost and recovery.

## Two ways to discover a change

```text
Polling:   Nubira -- any changes? --> Calendar API
                 <-- response -----

Webhook:   Calendar -- event ------> Nubira
           Nubira may fetch current details afterward
```

| Question | Polling | Webhooks |
| --- | --- | --- |
| Who starts the exchange? | Nubira checks the provider. | The provider sends a notification. |
| When do we notice a change? | After a successful check that includes it. | After successful notification delivery and processing. |
| What happens during quiet periods? | Checks may find nothing. | Typically no change notification is needed. |
| What can go wrong? | Checks fail, fall behind or miss changes outside a query window. | Notifications may be delayed, repeated or missed. |
| What must the provider support? | A suitable read/change-query operation. | Relevant events and a delivery mechanism. |

Neither option guarantees immediate synchronization. The provider's event coverage, retry policy, retention and ordering rules matter. For a concrete provider example, [Stripe's webhook documentation](https://docs.stripe.com/webhooks) explains duplicate handling and event ordering; do not assume another API makes the same guarantees.

## Design for the inconvenient sequence

Suppose a booking is changed to 11:00, then to 12:00. Nubira receives the notification about 12:00 first and an older notification later.

If every notification blindly overwrites the booking, the final state could become 11:00. Engineering might compare versions, fetch authoritative current state, or use another approach supported by the provider. Product must decide which system owns the appointment time and how conflicts are communicated.

A repeated notification should also not send a second customer email or recreate the booking. The event's identity can help detect redelivery, but two different events can describe related business changes. Discuss the operation's [idempotency](idempotency-for-product-managers.md), not only the transport.

## Agree on freshness and recovery

Replace “real time” with a user-relevant expectation: how stale can the displayed appointment be before a clinic is likely to act on incorrect information? The team can then assess a realistic target under normal operation and a recovery experience during outages.

A hybrid can make sense: use notifications for prompt updates and periodic reconciliation to compare authoritative records and repair gaps. This is additional work, not a guarantee that all conflicts disappear.

| Situation | Product behavior to define |
| --- | --- |
| Connected calendar is temporarily unavailable | Show the booking's local state and the synchronization state separately. |
| Credentials are revoked | Tell the appropriate administrator how to reconnect. |
| Connection returns after a long outage | Define how far back to reconcile and how to handle deletions. |
| Notifications arrive repeatedly | Prevent duplicate business effects. |
| Provider supplies only partial event data | Confirm what must be fetched before updating the product. |

Engineering should verify webhook authenticity using the provider's supported mechanism. A publicly reachable endpoint must not treat every incoming payload as a trusted calendar change.

## Questions to take to integration discovery

Which relevant changes generate events? Can we recover changes that happened while disconnected? Is ordering guaranteed for the resource we care about? What does the provider consider delivery success versus completed processing?

For recovering a large backlog, continue with [pagination and rate limits](pagination-and-rate-limits-for-product-managers.md). For asynchronous completion states, see [synchronous vs asynchronous processing](../Software-Architecture/synchronous-vs-asynchronous-processing-for-pms.md).

## Related resources

- [API Integration Discovery Checklist for Product Managers](api-integration-discovery-checklist.md) — Turn provider unknowns into a scoped decision with evidence and owners.

---

### Plan the whole synchronization experience

**APIs for Product Managers** — follow Nubira through the capabilities, limitations and product decisions behind connected systems.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
