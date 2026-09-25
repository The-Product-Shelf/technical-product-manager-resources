# REST vs GraphQL for Product Managers

## Compare the product request first

Nubira wants a receptionist's screen showing an appointment, practitioner and calendar-sync status. Would REST or GraphQL make that easier? The useful answer depends on the interface the provider offers, its permissions and how the screen behaves under load or partial failure.

REST is an architectural style; many APIs described as REST expose resources through HTTP operations. GraphQL is a query language and execution model built around a typed schema. It is not a database, and adopting it does not replace the underlying storage or automatically improve performance.

## What changes for this screen?

| Concern | Resource-oriented HTTP API | GraphQL API |
|---|---|---|
| Data selection | Provider defines response representations; some APIs offer field selection or expansion | Client requests fields exposed by the schema |
| Related information | May need several operations or an aggregation endpoint | May request related fields in one operation if the schema supports them |
| Caching | Can use HTTP caching where semantics and headers allow | Often needs query-aware or application-level caching; HTTP use alone does not settle it |
| Cost control | Limits may be per request, endpoint or workload | Limits may also consider query depth, complexity or resolved work |
| Access | Must authorize each protected operation and resource | Must authorize protected data and actions regardless of requested fields |
| Change management | Contract evolution and retirement need a policy | Schema evolution and field deprecation still need a policy |

These are tendencies, not guaranteed properties. A well-designed REST endpoint can return exactly the screen's data. A GraphQL query can still trigger expensive downstream work or encounter unavailable dependencies.

## One network request is not one unit of work

Imagine the screen needs ten appointments and their practitioner details. GraphQL may reduce client round trips, but resolving each appointment separately could still cause repeated backend reads. Engineering can assess batching and resolver behavior. Product should ask about measured screen latency, failure rate and cost under the expected workload rather than counting URLs.

Pagination also remains necessary. Asking for every historical appointment and all nested details in one query may exceed limits. Define how the interface loads more data and communicates incomplete results. See [pagination and rate limits](pagination-and-rate-limits-for-product-managers.md).

## Design partial results deliberately

A GraphQL execution result can contain usable data alongside errors; exact response and HTTP-status behavior depends on the server and failure. The screen might have the appointment time but no synchronization status. It should not quietly turn “unknown” into “synced.” A REST-based screen assembling several responses can face the same product problem.

Define which information is essential to act and which can be temporarily unavailable. Neither interface style decides the right fallback or permission model. The [API access guide](../Security/api-keys-vs-oauth-for-product-managers.md) explains the separate authorization concerns.

## A decision record worth keeping

State the required screen data, supported provider capabilities, measured constraints and ownership. Prefer the available interface that meets those requirements with manageable complexity. Using both styles can be reasonable when they serve different consumers; it is not a maturity ladder.

Ask: which queries are supported, who controls the schema, how are failures represented, and how will consumers learn about [contract changes](api-versioning-and-deprecation-for-product-managers.md)? Start with [API terminology](api-terminology-for-product-managers.md) if these boundaries are unfamiliar.

## Primary reference

[GraphQL's official learning guide](https://graphql.org/learn/) explains schemas, operations and execution. [GraphQL response documentation](https://graphql.org/learn/response/) describes data and error responses. Validate each provider's actual behavior rather than assuming every GraphQL implementation has identical capabilities.

---

### Evaluate interfaces through real workflows

**APIs for Product Managers** — explore Nubira’s integration decisions and learn to connect API capabilities with the experience a product needs.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
