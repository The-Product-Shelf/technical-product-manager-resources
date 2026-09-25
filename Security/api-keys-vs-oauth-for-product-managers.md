# API Keys vs OAuth for Product Managers

## Compare access models, not interchangeable objects

An API key is a credential used by an API under that provider's rules. OAuth is an authorization framework through which a client obtains access tokens. An OAuth access token is also a credential. “API key versus OAuth” is useful shorthand, but a key and an authorization framework are not the same kind of thing.

Neither label alone establishes security. Scope, storage, expiry, revocation and enforcement matter in both models.

## Quick comparison

| Dimension | API-key model | OAuth model |
|---|---|---|
| Common use | Identify or authorize an application, project or service | Grant a client bounded access to protected resources |
| User delegation | May require a provider-specific arrangement | Supports delegated access; some flows involve user approval |
| Machine access | Common | Also supported, for example through client credentials |
| Permissions | May be broad or scoped, depending on the API | Scopes and resource policies constrain access, depending on the service |
| Lifecycle | Rotation, expiry and revocation are provider-specific | Access tokens and sometimes refresh tokens have distinct lifecycles |
| PM concern | Safe onboarding and credential replacement | Consent, account connection, permission changes and disconnection |

OAuth does not always require a human consent screen. It is also not, by itself, a user authentication protocol; OpenID Connect adds an identity layer for sign-in use cases. Confirm what the actual integration implements.

## Worked scenario: a reporting integration

Klyvero wants customers to connect an external reporting service to their workspace. A shared credential with unrestricted access could expose more data than the integration needs. A better product requirement is specific: the reporting service can read agreed reporting data for the connected workspace, while write actions and other workspaces remain inaccessible.

That requirement may be implemented through OAuth scopes and resource checks, or through a suitably scoped service credential if the provider supports it. The mechanism must match the provider's capabilities and Security's assessment; a consent screen alone does not prove least privilege.

```text
Customer connects reporting service
  -> access granted for an agreed scope
  -> service reads permitted workspace data
  -> customer disconnects
  -> future access is revoked under the defined lifecycle
```

## Design the whole connection lifecycle

Make the connected workspace and permissions visible. Explain what changes when a person leaves the company or loses the authority to approve the integration. An integration owned by a departing employee may need transfer, reauthorization or removal, depending on its access model.

“Disconnect” also needs a precise meaning. Revoking future access does not necessarily delete data already copied to the reporting service. Revoking a refresh token may prevent renewal while a previously issued access token remains valid for a time. Define behavior with Engineering and Security rather than promising immediate universal removal.

Credential rotation should avoid unnecessary downtime while limiting old-credential exposure. Do not put long-lived secrets in public repositories, client-side examples or support tickets. Installation instructions should use placeholders and approved secret-handling mechanisms.

## Questions before committing the feature

- Whose authority does the integration use: a person, service account or application?
- Which resources and actions are permitted, and where are they enforced?
- What happens after permission changes, expiry or revocation?
- Can a workspace administrator inspect and remove connections?
- How will failed renewal or revoked access appear to the customer?

Use [authentication vs authorization](authentication-vs-authorization.md) for the core distinction, [sessions](sessions-for-product-managers.md) for ongoing access, and [HTTP status codes](../APIs/http-status-codes-for-product-managers.md) for interpreting failures under the API's contract.

## Security reference

[IETF RFC 9700: OAuth 2.0 Security Best Current Practice](https://www.rfc-editor.org/rfc/rfc9700.html) describes OAuth security considerations. The correct flow and controls should be reviewed by Security and Engineering.

---

### Ask better questions about connected access

**Security for Product Managers** — follow Klyvero’s security scenarios to connect identity and authorization choices with clear product behavior.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/security-for-product-managers/)
