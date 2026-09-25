# Pagination and Rate Limits for Product Managers

How do we import a customer's complete history without treating the first response as the whole dataset?

Nubira is a fictional booking platform. A clinic wants to import 12,000 appointments. An API returns at most 100 per response and restricts request volume. Even before implementation, Product needs to define progress, completeness and what happens if the import is interrupted.

## Read the collection in portions

| Term | Meaning | What to clarify |
| --- | --- | --- |
| Page size | Maximum or requested number of records in a response. | The provider may return fewer records. |
| Offset/page pagination | A position or page number selects the next portion. | Changes to the collection can shift positions. |
| Cursor pagination | A provider-issued marker identifies where to continue. | Markers can expire; stable snapshots are not guaranteed. |
| Next link/token | The documented continuation mechanism. | Follow the contract rather than guessing a URL or stopping rule. |
| Rate limit | A restriction on request volume over a period. | Scope may be account, credential, endpoint or another dimension. |
| Concurrency limit | A cap on work in flight at once. | This is different from requests allowed per minute. |

GitHub documents a concrete [pagination scheme](https://docs.github.com/en/rest/using-the-rest-api/using-pagination-in-the-rest-api) and several [rate-limit behaviors](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api). Providers differ, so these are examples rather than a universal protocol.

## A rough estimate is not a completion promise

With 12,000 records and 100 records on every page, Nubira needs at least 120 page requests. Under an illustrative limit of 60 requests per minute, those pages consume two minutes' worth of quota.

That is not a two-minute ETA. Response time, quota already in use, detail requests per appointment, retries and allowed bursts affect elapsed time. If each imported appointment requires a second API request, the request budget changes dramatically.

Ask Engineering which operations the estimate includes before promising an import duration to a clinic.

## Decide what “complete” means

Appointments can change while Nubira is paging through them. If the API does not provide a consistent snapshot, page 1 and page 120 may reflect different moments. Cursor pagination alone does not solve this.

A proposed import contract might use a defined cutoff and a later change-reconciliation pass. Whether that works depends on filters, update timestamps, deletion records and the provider's guarantees. Confirm what evidence demonstrates that all eligible records were processed.

Keep separate counts for records discovered, imported, skipped and failed. “100% of fetched pages processed” is not proof that every appointment was fetched.

## Make interruption recoverable

```text
Fetch portion -> Process records -> Save progress
                        |
                 Report failed items
                        |
            Continue or resume safely
```

This conceptual flow is not a prescribed transaction design. The checkpoint and write behavior must tolerate a crash between steps. Reprocessing an already imported appointment should not create a duplicate; use stable source identifiers and the integration's supported [idempotency strategy](idempotency-for-product-managers.md).

For a rate-limit response, the client may need to respect `Retry-After` or another documented reset signal. Uncontrolled parallel retries can delay the import further. See [timeouts and retries](api-timeouts-and-retries-for-product-managers.md).

## Product acceptance questions

- Can the clinic leave the screen and return to an accurate progress state?
- What happens when the continuation token expires or permission is revoked?
- Does retrying failed items avoid repeating successful business actions?
- Can Support distinguish “still processing” from “stuck” and “complete with failures”?
- What checks establish completeness when source data changes during the import?

---

### Scope imports with the API contract in view

**APIs for Product Managers** — explore Nubira’s API documentation, limits and integration requirements in more depth.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/apis-for-product-managers/)
