# HTTP Methods for Product Managers

What does an API let your product do with a resource? The address alone is not enough: the HTTP method is part of the operation.

Nubira is a fictional booking platform. Reading an appointment, changing its time and canceling it may involve the same appointment identifier, but they have different consequences for users and for retry behavior.

## Quick Reference

| Method | Typical intent | Illustrative Nubira operation | PM consideration |
| --- | --- | --- | --- |
| GET | Retrieve a representation. | Read appointment 4821. | Which fields and permissions apply? |
| POST | Submit data for resource-specific processing. | Create an appointment or request a cancellation. | Does repeating the request repeat the action? |
| PUT | Create or replace the target resource's state with the supplied representation. | Replace an editable appointment representation. | Which fields must the caller supply? |
| PATCH | Apply a set of changes. | Change the appointment's start time. | How are omitted fields, conflicts and repeated changes handled? |
| DELETE | Remove the association with the target resource. | Remove a draft appointment resource. | Does the business require cancellation, history or another action instead? |

These are protocol intentions, not a promise that every API supports each operation. A provider may expose cancellation as a POST action because it triggers a workflow. DELETE does not itself promise physical deletion from backups or every related system. Consult [HTTP method semantics](https://www.rfc-editor.org/rfc/rfc9110.html#section-9.3) and [PATCH semantics](https://www.rfc-editor.org/rfc/rfc5789.html).

## Safe and idempotent are different

**Safe** means the requested semantics are read-only. Logging or usage accounting can still happen. GET is safe; using it for a destructive action would violate that expectation.

**Idempotent** means repeating an identical request has the same intended effect as performing it once. HTTP defines PUT and DELETE as idempotent in their intended semantics, although implementations and external side effects still need to be verified. POST and PATCH do not guarantee idempotency. A particular POST API may provide an idempotency mechanism.

Identical effects do not require identical responses. A first DELETE might succeed, while the next reports that the resource is absent. Check the actual API contract rather than selecting a retry policy from the verb alone. See the [idempotency guide](idempotency-for-product-managers.md).

## Example: moving a booking is more than updating a field

A clinic asks to move Laura's appointment from 10:00 to 11:00. A PATCH that sets `start_time` sounds sufficient, but Product still needs to clarify:

- Must availability be checked again?
- Does the existing price remain valid?
- Is the calendar update part of the same result or a later synchronization?
- What happens if another employee edits the appointment first?

A “set start time to 11:00” operation can behave differently from “move this appointment forward by one hour.” Repeating the latter could move it twice unless the API prevents that effect.

For a proposed PUT operation, ask whether sending only `start_time` would omit required fields or reset other values. The contract determines the accepted representation; the method name is not a field-level specification.

## Read the operation before committing to scope

When evaluating a provider, record the method and path together, the required inputs, permissions, success result and relevant failure cases. An API that lets us GET appointments may not let us create or modify them. A successful response to one operation does not establish that a whole integration has completed.

Use the [status-code reference](http-status-codes-for-product-managers.md) to interpret the reply and [API keys vs OAuth](../Security/api-keys-vs-oauth-for-product-managers.md) to understand the access model.

A useful question for Engineering is: “Which documented operation achieves the customer's intended outcome, and what can happen if we repeat it or another change happens first?”

---

### Connect methods to integration decisions

**APIs for Product Managers** — follow Nubira through requests, responses and API documentation to understand what an integration can actually deliver.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
