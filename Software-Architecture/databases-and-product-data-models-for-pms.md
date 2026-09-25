# Databases and Product Data Models for PMs

## Define the meaning before choosing the database

Nómada adds saved baskets and order history. Before debating relational versus document storage, clarify what an order represents, how it relates to a customer and what must remain true after prices or accounts change. Those decisions shape the product regardless of storage technology.

A **data model** describes entities, relationships and rules. A **database** stores and retrieves data using particular capabilities and trade-offs. One model can have several physical implementations, and one product may use multiple stores for different workloads.

## Sketch identity and relationships

```text
Customer -> Orders -> Order lines -> Product reference
                        |
                  Purchased quantity and price
```

This sketch describes relationships, not deployment boundaries. One customer can have many orders, and an order can have several lines. Product references connect to today's catalog; purchased quantity and price describe the transaction at the time of purchase.

| Question | Nómada example | Consequence if left ambiguous |
|---|---|---|
| What identifies the entity? | Stable order ID, separate from display number | Renaming or merging can break references |
| What is the relationship? | One order contains many lines | Joining tables may multiply order rows |
| What must be unique? | Provider transaction reference within its defined scope | Duplicate events may create duplicate orders |
| What can be missing? | Guest order has no registered-customer ID | Missing identity may be valid, not corruption |
| What is historical? | Price paid versus current catalog price | Updating the catalog could rewrite the apparent purchase |
| What changes together? | Inventory reservation and order acceptance | Partial updates may violate the intended promise |

## Compare storage by access patterns

Relational stores commonly express relationships and constraints explicitly. Document stores can group related data for convenient retrieval, while search indexes support search-oriented access. These categories overlap in capability; “NoSQL has no schema” and “relational means slow” are not useful decision rules.

Ask which operations are frequent, which constraints must be enforced, what consistency is needed and how data changes over time. Engineering should evaluate the actual technology, configuration and workload. An analytics warehouse or search index may lag behind the transactional source, so its result may not be suitable for a real-time checkout decision.

## Worked scenario: the customer deletes an account

Does deleting a Nómada profile remove every order, anonymize the customer association, restrict visibility, or trigger a reviewed retention process? Historical purchase records, support needs and data-handling requirements may differ. Product must identify the use cases and involve Security and the appropriate data/privacy owners; a database cascade is not a complete retention policy.

Also define what happens to guest orders later linked to an account. Matching only by email can attach records to the wrong identity if ownership changes. Agree stable identity and verification rules instead of assuming a convenient field is globally unique forever.

## What a PM should bring to the design review

Bring example records, relationship cardinalities, lifecycle transitions and invariants such as “one accepted payment must not create two orders.” Include missing and conflicting examples, not only the happy path. Ask which system owns each fact and how other copies are reconciled.

[SQL JOINs](../SQL/sql-joins-explained-for-product-managers.md) show why relationship grain affects counts. [Duplicates and NULLs](../SQL/finding-duplicates-and-handling-nulls-for-pms.md) helps inspect anomalies, and the [metrics reference](../Product-Metrics/product-metrics-cheat-sheet.md) connects entities with measurement units.

## Primary reference

[Microsoft's data-model overview](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/data-store-overview) compares storage models and their access-pattern trade-offs.

---

### Connect stored data with product behavior

**Software Architecture for Product Managers** — follow Nómada’s architectural decisions and understand how relationships, ownership and consistency shape the experience.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
