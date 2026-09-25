# HTTP Status Codes for Product Managers

Quick reference for understanding API responses and deciding what should happen next in the product.

Imagine a customer books an appointment. Your product saves the booking, then tries to add it to an external calendar. The booking succeeds, but calendar synchronization fails. Should the screen say “Booking failed”?

To answer that, we need to know which operation succeeded, which failed, and what the customer can do next. An HTTP status code gives us one part of that picture.

## Start with the first digit

| Family | Meaning |
| --- | --- |
| 1xx | Informational; processing continues. |
| 2xx | Successful response. |
| 3xx | Redirection; further action is needed to complete the request. |
| 4xx | Client error. |
| 5xx | Server error. |

“Client” means the software making the request. A 4xx response does not automatically mean the person using your product did something wrong. See the [HTTP status code overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status).

## The codes worth recognizing

Use the meaning column to orient yourself, then check the API documentation and error details. The product questions below are prompts for a conversation, not automatic rules for handling every response.

### Successful responses

| Code | Meaning | Product question |
| --- | --- | --- |
| **200** | Request succeeded. | What did this particular request confirm? |
| **201** | Resource created. | Do we have the identifier needed to find it later? |
| **202** | Accepted; processing is incomplete. | How will the user learn the final outcome? |
| **204** | Success with no response content. | Can the interface confirm completion without returned data? |

A **202 is not a promise of eventual success**. Design a pending state and a way to discover completion or failure. Definitions: [HTTP Semantics, successful responses](https://www.rfc-editor.org/rfc/rfc9110.html#section-15.3).

### Requests the server cannot fulfill

| Code | Meaning | Product question |
| --- | --- | --- |
| **400** | Request rejected as a client error. | Can the person fix an input, or is our integration sending something invalid? |
| **401** | Valid authentication credentials are missing. | Does the connection need renewal or reconnection? |
| **403** | Server refuses the request. | Is this action unavailable under the current permissions or policy? |
| **404** | Resource not found, or its existence is concealed. | Are we using an outdated reference or requesting something unavailable to this account? |
| **409** | Conflict with current resource state. | Should we refresh availability before asking the user to act again? |
| **422** | Content understood, but instructions cannot be processed. | Can we explain the rejected value or business rule? |
| **429** | Request rate limit exceeded. | How will delayed work affect the experience during busy periods? |

A 403 often involves permissions, but does not by itself prove that authentication succeeded. APIs also differ in their use of 400 and 422 for validation failures. Read the documented error details before choosing a user message. Definitions: [HTTP Semantics, client errors](https://www.rfc-editor.org/rfc/rfc9110.html#section-15.5).

For **429**, the response may include `Retry-After`, indicating how long to wait. Agree with Engineering how retries, waiting time, and accumulated work will be handled; repeatedly sending the same request immediately can make the problem worse. See [the 429 specification](https://www.rfc-editor.org/rfc/rfc6585.html#section-4).

### Server failures

| Code | Meaning | Product question |
| --- | --- | --- |
| **500** | Unexpected server failure. | Which part of the customer's task is affected? |
| **502** | Gateway received an invalid upstream response. | Which dependency should the investigation follow? |
| **503** | Service temporarily unavailable. | Can we preserve the user's progress while service recovers? |
| **504** | Gateway waited too long for an upstream response. | Do we know whether the underlying operation completed? |

Definitions: [HTTP Semantics, server errors](https://www.rfc-editor.org/rfc/rfc9110.html#section-15.6). A gateway is an intermediary server communicating with another server to fulfill the request.

## Example: the booking exists, but the calendar is not updated

Nubira is a fictional booking platform used by clinics and other appointment-based businesses. Consider this illustrative flow; it does not describe a specific calendar provider's API contract.

1. Laura books an appointment for Tuesday at 10:00.
2. Nubira's booking API returns **201** with the new booking's identifier.
3. Nubira tries to create the corresponding event in the clinic's connected calendar.
4. The calendar API returns **401**. Investigation confirms that the connection needs the clinic administrator to reconnect it.

If Nubira displays “Booking failed. Please try again,” Laura may create another booking even though the first one already exists.

Instead, the patient can see the confirmed appointment. The clinic administrator can see a separate message:

> The appointment is saved in Nubira, but it hasn't synced to your calendar. Reconnect your calendar to resume synchronization.

That message creates a product requirement: what happens to appointments waiting to sync after reconnection? We need to define recovery as well as the error message.

Keeping booking status and synchronization status separate also gives Support a more useful answer when the clinic reports a missing calendar event.

## Before adding a “Try again” button

A missing response does not prove that nothing happened. Nubira might send a request to create an event, the external system might create it, and the response might fail to reach Nubira. Some connection failures produce no HTTP status code at all.

For operations that create bookings or take payments, agree with Engineering how the product checks the outcome and prevents duplicates. One mechanism is **idempotency**: allowing a repeated request to have the same intended effect as making it once. Its availability depends on the operation and API contract. See [HTTP Semantics, idempotent methods](https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.2).

From Product, specify what users should see while the outcome is uncertain. “We're checking your booking” may be more appropriate than a failure message that encourages another submission, provided the product actually performs that check.

## Questions worth asking Engineering

- Which operation does this response describe, and what has already been saved?
- Which errors can users resolve, and which require our team or the provider?
- If work is accepted for later processing, how do we track its final outcome?
- When is retrying safe, and how do we prevent duplicate bookings or charges?
- What identifiers and status information will Support need to investigate a reported failure?

The useful outcome is a clear recovery path: users understand what happened, what remains pending, and whether they need to act.

---

### Want to go deeper?

This resource is part of The Product Shelf's free Technical Product Management library.

**APIs for Product Managers** follows Nubira through requests, responses, authentication, API documentation, and integration decisions.

→ [Explore the book at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
