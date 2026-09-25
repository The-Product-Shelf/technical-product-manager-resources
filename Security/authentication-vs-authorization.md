# Authentication vs Authorization

Why being logged in is not enough to access a resource.

Klyvero is a fictional project management product used by several organizations. Laura is an administrator in one organization and a regular member in another. Her identity stays the same, but the actions she should be allowed to perform depend on the context.

## Two different questions

| | Authentication | Authorization |
| --- | --- | --- |
| Question | Who is making this request? | May this identity perform this action on this resource? |
| Example | Laura proves her identity through the company's sign-in flow. | Klyvero checks whether Laura may export members of organization A. |
| Typical product decisions | Sign-in options, session duration and account recovery. | Roles, resource ownership, organization boundaries and exceptions. |
| What it does not establish | Permission to access every resource. | That every other action by this person should be allowed. |

OWASP separates these responsibilities in its [authorization guidance](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html). A successful login should not be treated as a blanket access decision.

```text
Request: export organization A's members
                  |
          Authenticate Laura
                  |
                  +-- Not established --> Do not proceed
                  |
            Identity established
                  |
     Authorize this export for organization A
                  |
                  +-- Denied -----------> Do not export
                  |
                Allowed
                  |
             Perform export
```

This is a conceptual decision flow. Being recognized as Laura gets the request to the permission check; it does not decide the result of that check.

## Turn “admins can export” into a policy

Suppose Klyvero is adding a member export. The following is an illustrative policy to discuss, not a default for every SaaS product.

| Identity and context | Export organization A's members? | Reason |
| --- | --- | --- |
| Active administrator of A | Yes | The requested action is within the assigned role and organization. |
| Regular member of A | No | Membership alone does not grant export permission. |
| Administrator of B, without membership in A | No | Administrative rights are scoped to B. |
| Former administrator of A | No | A previous role does not imply current access. |
| Not signed in | No | The request lacks the required authenticated identity. |

If Laura is an administrator of A and a member of B, she can export A's members under this policy, but not B's. A global label saying “Laura is an admin” would lose the distinction.

Some products need more detail than roles: access may depend on a project relationship, the record's owner, a specific permission or another attribute. Product should describe the behavior; Engineering and Security should choose and assess the enforcement model.

## Hiding a button is not enforcement

The interface can hide the export button when Laura is in organization B. That helps explain what she can do, but requests can reach the backend without using that screen.

The server must verify the relevant permission for the requested action and resource. Changing an organization ID in a request must not expose another organization's data. OWASP recommends validating permissions on every request and denying access by default unless explicitly allowed.

For acceptance criteria, describe both the permitted path and the denied paths. “Laura can export organization A” should be accompanied by “Laura cannot export organization B under her member role.”

## Access changes over time

Now imagine an export runs in the background. Laura starts it as an administrator, then loses that role before downloading the result.

The product needs a clear rule for the later download. It also needs a response when someone is removed from an organization while a session remains open. Revocation is not complete merely because the management screen shows a new role.

Work with the team to identify the affected sessions, tokens, background work and download links. Define how quickly access must change and verify that the implementation can meet the expectation. A downloaded file may remain outside the application's control.

## Read errors as clues

A 401 commonly points to missing or invalid authentication; a 403 indicates refusal. An API may return 404 to conceal a resource's existence. These codes help investigation, but the API contract and error details determine the user-facing response. See the [HTTP status codes guide](../APIs/http-status-codes-for-product-managers.md).

## Questions worth asking Engineering and Security

- Are roles scoped to an organization, project or the whole product?
- Which action and resource does each permission cover?
- Are the same rules enforced through the interface, API and downloads?
- What happens to existing access after a role or membership changes?
- Which allowed and denied cases should be demonstrated before release?

For related vocabulary, use the [security glossary](security-glossary-for-product-managers.md).

## Related resources

- [Sessions for Product Managers](sessions-for-product-managers.md) — Specify expiry, logout and revocation across devices and organizations.
- [API Keys vs OAuth for Product Managers](api-keys-vs-oauth-for-product-managers.md) — Choose an access model for service and user-delegated integrations.

---

### Follow access through the user lifecycle

This resource is part of The Product Shelf's free Technical Product Management library.

**Security for Product Managers**

Explore authentication, sessions, account recovery and permissions, including the product questions that arise when access changes.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/security-for-product-managers/)
