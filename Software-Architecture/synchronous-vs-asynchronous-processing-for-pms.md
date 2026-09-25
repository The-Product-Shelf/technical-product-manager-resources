# Synchronous vs Asynchronous Processing for PMs

When should a user wait for a result, and when can the product acknowledge work that will finish later?

Nómada is a fictional online shop. A customer asks to export a year of orders. Preparing the file may take longer than a comfortable wait in a browser. Moving the work to the background changes the product's states, not just its response time.

## Two different completion experiences

| Approach | What the caller does | Example | Product responsibility |
| --- | --- | --- | --- |
| Synchronous | Waits for the operation's result before proceeding. | Ask for the current order total. | Define acceptable waiting and what happens on failure. |
| Asynchronous | Handles completion separately from the initial request. | Request an export, then retrieve it later. | Define progress, notification, expiry and recovery. |

A synchronous request can still use concurrent work internally. An asynchronous API does not mean the task will be faster; it means the caller and completion are decoupled. Architecture and interface behavior should be discussed separately.

## Give the job a lifecycle

For Nómada's export, a proposed lifecycle could be:

```text
Request accepted -> Pending -> Running -> Succeeded
                                  |
                                  +------> Failed
```

A cancellation request is another action; it may race with completion. Do not show “Canceled” merely because the user clicked the button if the system has not confirmed that outcome.

An API may return 202 to acknowledge acceptance and provide a way to check status. The status query itself can succeed while reporting that the job failed. HTTP success and the business job's state are different layers. Microsoft's [asynchronous request-reply pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/async-request-reply) describes one implementation approach.

## Example: the export finishes after access changes

An administrator requests the order export, closes the browser and later loses export permission. The job completes overnight. Should the emailed link still grant access?

Product must specify who may retrieve the result when it is ready, how permission is checked, how long it remains available and whether the file contains data outside the original scope. Background execution does not remove the need for authorization at download time.

Also define what the file represents: orders at request time, orders at processing time, or a documented cutoff. A progress indicator cannot resolve an ambiguous data snapshot.

## Acceptance criteria that describe the experience

| State or event | Behavior to agree |
| --- | --- |
| Accepted | Provide a persistent job reference and explain what is pending. |
| Running | Let the user leave and return; avoid invented percentage progress. |
| Succeeded | Make the result discoverable to an authorized requester. |
| Failed | Distinguish a recoverable failure from one requiring changed input or support. |
| Repeated request | Clarify whether it retrieves the existing job or starts another. |
| Result expired | Explain how to request a fresh export. |

If progress cannot be measured honestly, a meaningful state and last update can be more useful than a fabricated “90%.” If notifications fail, the result should still be discoverable through the product where appropriate.

## Questions for Engineering

Which steps must finish before acknowledgment? What happens if a worker crashes? Can the same work be submitted twice? How long can a pending job remain pending before it needs intervention? Which data and permission checks occur at request time and at completion?

Continue with [queues and message brokers](queues-and-message-brokers-for-pms.md) for how background work is delivered, and [sessions](../Security/sessions-for-product-managers.md) for access changes over time.

---

### Connect processing choices to the experience

**Software Architecture for Product Managers** — follow Nómada’s architecture decisions and the product trade-offs introduced by work that completes later.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
