# Software Architecture Glossary for PMs

A map of the components and constraints behind a product experience.

Nómada is a fictional online shop for travelers and remote workers. A customer sees a backpack, adds it to a cart and pays. To discuss changes to that experience, a PM needs to understand where the work happens and what the product depends on.

## The components

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Client** | Software that requests something from another system. | A mobile app and a web browser may use the same backend but offer different experiences. |
| **Server** | Software or a machine that handles requests. | A visible interaction can depend on several servers. |
| **Frontend** | The software presenting the experience and handling user interaction. | Adding a button may also require changes elsewhere. |
| **Backend** | The software performing work behind that interface, including business rules and data access. | Price, permission and stock rules need reliable enforcement beyond the screen. |
| **Service** | A component providing a capability to other components. | A service boundary tells you who owns a dependency; it does not automatically mean “microservice.” |
| **Database** | A system for storing and retrieving structured information. | Understand where the authoritative order or stock record lives. |
| **Cache** | A stored copy used to avoid repeating slower work. | Faster responses may come with temporarily outdated information. |
| **CDN** | A distributed network that delivers content from locations closer to users. | Product images and other assets can load faster; content updates need consideration. |
| **Queue** | A mechanism that holds work for later processing. | “Request received” can precede “task completed.” |
| **Dependency** | A component or provider your product needs for some behavior. | Its limits and failures can affect your product promise. |

## The properties and trade-offs

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Latency** | The time taken for a defined operation or response. | Define which part of the user's wait you are measuring. |
| **Throughput** | The amount of work processed per unit of time. | Completing more orders per minute is different from making one order faster. |
| **Scalability** | The ability to handle increasing workload, potentially by adding resources. | Ask which workload grows and what breaks first. |
| **Availability** | Whether a service can provide its intended function when needed, measured using a defined indicator. | “The site is up” may hide a broken checkout. |
| **Bottleneck** | The constraint currently limiting performance or capacity. | More servers may not help if another component is the constraint. |
| **Synchronous work** | Work where the caller waits for the result before proceeding. | Dependencies can add to the wait. |
| **Asynchronous work** | Work whose completion is handled separately from the initial request. | Define progress, completion, failure and recovery states. |
| **Consistency** | Rules about which data updates different readers can observe. | After an update, different screens may not immediately show the same state. |
| **Technical debt** | Design or implementation choices that create future costs or constraints. | Discuss specific consequences and remedies rather than treating all maintenance as the same thing. |

## Example: a fast catalog and a trustworthy checkout

A simplified Nómada flow might look like this:

```text
Customer → Storefront → Backend → Database
                            ↓
                      Payment provider
                            ↓
                Queue → Confirmation email
```

A cache could accelerate catalog browsing. But if a cached stock count says one backpack is available after the last unit has sold, the checkout still needs to resolve actual availability. Product should define what the customer sees if the item sells out between browsing and payment.

Sending a confirmation email through a queue can avoid making the customer wait for email delivery. It also introduces another state: the order is confirmed, but the email is pending or has failed. We should not describe an email failure as a failed purchase.

This is an illustrative architecture, not a prescription. Ask Engineering where these responsibilities live in your own system. For further technical context, see Microsoft's architecture guidance on [caching](https://learn.microsoft.com/en-us/azure/architecture/best-practices/caching).

## Questions worth asking Engineering

- Which components and providers does this user action depend on?
- What is the source of truth for price, stock and order status?
- Which steps must finish before we confirm success?
- Where do we accept stale information, and for how long?
- What measured constraint motivates the proposed architecture change?

For a comparison of deployment structures, continue with [Monolith vs Microservices](monolith-vs-microservices-for-pms.md).

---

### Connect the architecture pieces

This resource is part of The Product Shelf's free Technical Product Management library.

**Software Architecture for Product Managers**

Follow Nómada as it grows, introducing components when a product problem calls for them and examining the trade-offs each brings.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
