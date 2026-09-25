# API Terminology for Product Managers

A practical glossary for scoping integrations and following conversations with Engineering.

Nubira is a fictional booking platform. A clinic wants appointments created in Nubira to appear in its external calendar. “Connect the calendars” sounds like one requirement, but we need to define what can move between the systems, who permits it, and what happens when either system changes.

## The request and the agreement behind it

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **API** | An interface through which software interacts with other software under defined rules. | An API's existence does not guarantee that it supports the experience you want. |
| **Resource** | Something the API exposes, such as an appointment or calendar. | Map the resources to the objects users recognize. |
| **Endpoint** | A specific address through which an API can be accessed. Documentation often describes an operation using both its URL and method. | Check that the required operation exists for the relevant resource. |
| **HTTP method** | The requested action: for example, GET retrieves data and POST submits data for processing. | Reading appointments and creating them are different capabilities. |
| **Request** | A message asking the API to perform an operation. | Define what information the product must supply. |
| **Response** | The reply, including a status code and potentially data. | Decide what the product can conclude from that reply. |
| **Headers** | Metadata sent with a request or response, such as content type or credentials. | Some requirements sit outside the visible business data. |
| **Body / payload** | Content carried in a request or response. | Decide which appointment details actually need to be shared. |
| **JSON** | A common text format for structured data. | Reading field names and values helps you inspect examples without writing an integration. |
| **Parameter** | An input to an operation, such as an appointment ID or date filter. | Check how the API identifies and narrows the information needed. |
| **API contract** | The expected inputs, outputs, behavior and constraints of an interface. | Use it to assess scope, dependencies and changes. |

For the underlying HTTP vocabulary, see [HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110.html). For response codes and recovery decisions, use the [HTTP status codes guide](http-status-codes-for-product-managers.md).

## Terms that change integration scope

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Authentication** | Establishing the identity associated with a request. | A connected account may need to reconnect when credentials stop working. |
| **Authorization** | Deciding whether that identity may perform the requested action on the resource. | Access to one calendar does not imply access to every calendar. |
| **Pagination** | Returning a collection in portions. | The first response may contain only part of a customer's history. |
| **Rate limit** | A restriction on request volume over a period. | Imports and synchronization may need to wait during bursts of activity. |
| **Webhook** | An event notification sent to a configured destination. | Ask how missed, repeated or out-of-order notifications are handled. |
| **Polling** | Repeatedly checking for changes. | The checking interval affects freshness, request volume and cost. |
| **Idempotency** | Repeating an operation has the same intended effect as performing it once. | Safe retries matter when creating appointments or payments; confirm the API's actual guarantees. |
| **Versioning** | Identifying different versions of an API contract. | Consumers may need time to adopt changes. |
| **Deprecation** | Notice that a capability is being phased out. | Record migration responsibilities and any retirement date. |

Authentication and authorization are explored separately in [this guide](../Security/authentication-vs-authorization.md).

## Example: define what “sync” means

For an illustrative Nubira integration, we might agree to create an external event when a booking is confirmed, store the returned event ID, and use that ID when the booking changes.

That still leaves important decisions. If someone edits the external event, should Nubira change the booking? Can the provider notify us of that edit? If there are multiple calendars, which one receives the appointment? If the integration disconnects for a day, which changes should be recovered afterward?

These answers describe different products, even if each is labeled “calendar sync.” Capture the supported direction of updates and the source of truth for each field before estimating the integration. The provider's documentation and a technical investigation should confirm what is possible.

## Questions worth asking Engineering

- Which operations and permissions does this experience require?
- What data leaves Nubira, and what identifiers do we store in return?
- How do we discover changes and recover missed updates?
- What happens when requests are delayed, repeated or rejected?
- Which contract changes would require work on our side?

## Related resources

- [HTTP Methods for Product Managers](http-methods-for-product-managers.md) — Choose the operation and understand safe versus idempotent behavior.
- [Polling vs Webhooks for Product Managers](polling-vs-webhooks-for-product-managers.md) — Define freshness, delivery failure and reconciliation for calendar sync.
- [Pagination and Rate Limits for Product Managers](pagination-and-rate-limits-for-product-managers.md) — Plan a complete import under changing data and request limits.

- [REST vs GraphQL for Product Managers](rest-vs-graphql-for-product-managers.md) — Compare interfaces against a real screen, authorization and operating constraints.
- [API Integration Discovery Checklist for Product Managers](api-integration-discovery-checklist.md) — Turn provider unknowns into a scoped decision with evidence and owners.

---

### Explore an integration from start to finish

This resource is part of The Product Shelf's free Technical Product Management library.

**APIs for Product Managers**

Follow Nubira from requests and endpoints to documentation, authentication and the product decisions behind an integration.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
