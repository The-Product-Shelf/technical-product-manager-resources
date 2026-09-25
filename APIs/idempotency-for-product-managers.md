# Idempotency for Product Managers

How can a product retry an uncertain request without creating a second booking or charge?

Nubira is a fictional booking platform. A clinic submits a booking, waits, then sees a timeout. The server may already have created the appointment even though the confirmation did not arrive. A second attempt must be considered in terms of the same business intent, not just a second network message.

## One intention, more than one attempt

```text
Intent: create this booking once
    |
    +-- Attempt 1 --> Booking created; response lost
    |
    +-- Attempt 2 --> Recognize the same intent
                     Return the known outcome, if supported
```

An idempotent operation has the same intended effect when repeated as when performed once. The HTTP method provides some expectations, but a POST-based creation operation typically needs an explicit API mechanism to offer repeat protection. Read [HTTP idempotency semantics](https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.2).

An **idempotency key** is a common way to identify the operation across attempts. [Stripe's API](https://docs.stripe.com/api/idempotent_requests) is one example. Key scope, retention, parameter matching and responses depend on the actual provider contract.

## Define the boundaries with Engineering

| Question | Why it changes the product behavior |
| --- | --- |
| What business intent does a key represent? | Retrying one booking differs from deliberately booking a second appointment. |
| How long is the key remembered? | A retry after expiry may no longer be protected. |
| Must repeated requests have identical parameters? | Reusing the key for an edited booking may be rejected or mishandled. |
| What happens to simultaneous attempts? | Two fast clicks can overlap before either receives a response. |
| Which effects are covered? | Creating one booking does not automatically ensure one email or one downstream charge. |
| How can we look up the result? | Support needs a way to resolve uncertainty without creating another operation. |

These questions are requirements discovery, not a proposal for PMs to invent their own distributed transaction mechanism.

## Example: retry or a new booking?

Laura requests a Tuesday appointment at 10:00. The screen times out. Clicking “Check booking status” should help resolve that original request. If the system safely retries creation, it should preserve the identity of that intent under the API contract.

Now Laura changes the request to Wednesday at 11:00. Is that a new booking, a replacement for the first, or a reschedule after the first succeeds? Reusing a key without defining this transition can preserve the wrong intent. Issuing a new key immediately can create two appointments if Tuesday's booking already exists.

Design the uncertainty state: show what is being checked, prevent misleading success or failure claims, and define what the user can edit before the outcome is known. A disabled button can reduce accidental clicks, but it cannot prevent all retries from networks, clients or background workers.

## Idempotency is not exactly-once delivery

Requests or messages can still be delivered multiple times. The goal is to control the resulting business effect. That protection may end at a system boundary, expire after a period or exclude some side effects.

Do not treat “we use a key” as evidence that duplicates are impossible everywhere. Ask for a demonstration involving a lost response, repeated requests and a downstream failure. Recovery should also account for data that was already written.

Pair this guide with [timeouts and retries](api-timeouts-and-retries-for-product-managers.md), [HTTP methods](http-methods-for-product-managers.md) and [queues and message brokers](../Software-Architecture/queues-and-message-brokers-for-pms.md).

---

### Reason about failure as part of the integration

**APIs for Product Managers** — use Nubira’s request and response scenarios to connect API behavior with the booking experience.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
