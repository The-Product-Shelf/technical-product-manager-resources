# Monolith vs Microservices for PMs

What changes for product delivery when a system is deployed together or as separate services?

Nómada is a fictional online shop. Its catalog, checkout and order management began as one application. As the business grows, Engineering proposes separating some capabilities. Before attaching a migration to the roadmap, ask what current problem that separation would solve.

## The distinction

A **monolith** is an application deployed as a unit. It can still contain well-separated modules and be maintained by multiple teams.

A **microservices architecture** divides the application into services designed to be deployed independently. Those services communicate across boundaries and typically own their data. Independence takes deliberate design; putting code into separate repositories does not create it automatically.

The comparison below describes tendencies, not guarantees. Microsoft's [microservices architecture guidance](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices) discusses both the benefits and the operational challenges.

| Dimension | Monolith | Microservices | PM question |
| --- | --- | --- | --- |
| Delivery | One deployment unit can simplify coordination, but changes may share a release process. | Independent deployment is possible, but cross-service changes still need coordination. | Which changes currently wait for unrelated work? |
| Scaling | Replicating the application may scale more than the busy component needs. | Individual services can be scaled separately. | Is uneven demand creating a measured cost or capacity problem? |
| Reliability | A failure may affect the shared application. | Boundaries can contain some failures, while network dependencies create others. | Which user journeys remain usable when one component fails? |
| Data | Shared transactions can be simpler within one database. | Cross-service workflows need explicit coordination and recovery. | What happens if payment succeeds but order confirmation fails? |
| Operations | Fewer deployed components to configure and observe. | More services, interfaces and failure modes to operate. | Can the team diagnose and support this arrangement? |
| Ownership | Clear module ownership is possible. | Services can align ownership with business capabilities. | Who is responsible for each capability end to end? |

## Example: should Nómada split its checkout?

Suppose checkout and catalog changes repeatedly interfere with each other. There are several possible explanations: unclear module boundaries, slow tests, a shared release schedule, or genuinely different scaling needs.

Each explanation suggests different work. If tests are unreliable, creating a second service will not automatically make them reliable. If the catalog needs far more capacity than checkout, independent scaling may be worth investigating.

A useful proposal connects the change to evidence:

> Catalog updates are delaying checkout releases. We will first identify the shared dependencies and evaluate whether a stronger module boundary or a separate service would reduce that delay.

That gives Product a question it can help answer: how much is the current constraint costing users and delivery, and how will we know the change helped?

## Migration is product work too

Separating a service can require running old and new paths together, moving data, preserving API behavior and investigating mismatches. The effort can compete with feature delivery before it produces a benefit.

For Nómada, a gradual change might begin with a capability whose boundaries are already understood. A different team may find that improving a modular monolith meets its needs. Neither choice should depend on a presumed maturity ladder from “monolith” to “microservices.”

Before committing, agree on a small set of outcomes: fewer blocked releases, more predictable operation at peak demand, or reduced cost for a particular workload. Include the ongoing operational cost when evaluating the result.

## Questions worth asking Engineering

- What observed constraint are we trying to remove?
- Could clearer module boundaries or delivery improvements address it?
- Which customer journeys cross the proposed service boundaries?
- Who owns failures and support across those boundaries?
- How will we migrate, measure the benefit and limit disruption?

Need the underlying vocabulary? Start with the [architecture glossary](software-architecture-glossary-for-pms.md).

---

### Want to go deeper?

This resource is part of The Product Shelf's free Technical Product Management library.

**Software Architecture for Product Managers** explores these topics through practical product scenarios.

→ [Explore the book at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
