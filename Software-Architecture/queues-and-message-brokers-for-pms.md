# Queues and Message Brokers for PMs

What does it mean when Engineering says, “We'll put it on a queue”?

Nómada is a fictional online shop. After confirming an order, it needs to notify a warehouse and send a receipt. Some work can continue in the background, but a queue is not a promise that every downstream action has completed.

## The minimum vocabulary

| Term | Meaning | PM question |
| --- | --- | --- |
| Producer | A component submitting a message. | Which business event causes submission? |
| Queue | A holding mechanism for work awaiting consumption. | How old is the oldest pending item? |
| Consumer/worker | A component processing messages. | What business outcome counts as success? |
| Broker | Infrastructure that routes or holds messages under configured rules. | What delivery and storage guarantees are configured? |
| Acknowledgment | A signal that a delivery has been handled under the messaging protocol. | At what point does the consumer acknowledge? |
| Dead-letter handling | A way to separate messages that cannot be processed normally. | Who investigates and decides whether to replay them? |
| Backlog | Work waiting to be processed. | Is it shrinking or growing faster than the team can process it? |

A broker may support queues, publish/subscribe or other models. With competing consumers on one work queue, messages are typically distributed among workers; publish/subscribe can give multiple consumers their own view. These are different product-delivery needs.

## Trace the business outcome

```text
Order confirmed -> Message submitted -> Warehouse worker
                                              |
                                     Warehouse notified
```

If submission succeeds but the worker is unavailable, the order may be confirmed while the warehouse remains uninformed. What should the dispatch estimate say? At what age should an unprocessed order require attention?

A queue can absorb a temporary burst, but it cannot create unlimited capacity. If orders arrive faster than workers finish for a sustained period, waiting time grows. Queue length and oldest-message age answer different questions: a short queue can still contain one long-stuck order.

## Prepare for repeated and reordered work

A worker might notify the warehouse and then fail before acknowledging the message. Redelivery could repeat the notification. Confirm how the consumer prevents a repeated message from creating a second shipment.

Even if messages enter a queue in order, parallel consumers, retries or separate queues may affect processing order. A cancellation can race with a dispatch instruction. Product should define the permitted order states and how conflicts are resolved; Engineering determines the coordination required.

[RabbitMQ's reliability guidance](https://www.rabbitmq.com/docs/reliability) provides concrete examples of redelivery and acknowledgment. Different brokers and configurations offer different guarantees. “Exactly once” needs a defined boundary and should not be assumed to cover external side effects.

## Failed work needs an owner

Suppose an order contains a warehouse code that no longer exists. Repeatedly retrying the unchanged message may never work. Dead-letter handling can separate the item, but moving it aside does not fulfill the order.

Agree on an investigation owner, an actionable record and a safe replay process. Replaying a fixed message should account for any part that already succeeded. Customer Support needs the order's business state, not just “the queue is green.”

For a launch review, ask the team to demonstrate a worker outage, a duplicate delivery and one permanently invalid message. Observe how these situations become visible and recoverable.

See [asynchronous processing](synchronous-vs-asynchronous-processing-for-pms.md) for job states, [idempotency](../APIs/idempotency-for-product-managers.md) for repeated effects, and [incident investigation](../Observability/production-incident-investigation-for-pms.md) for escalation.

---

### Understand what background delivery adds to a product

**Software Architecture for Product Managers** — explore Nómada’s dependencies, asynchronous work and recovery trade-offs beyond the queue itself.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
