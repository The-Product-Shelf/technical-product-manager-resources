# Sessions for Product Managers

## A session connects requests to a signed-in context

After authentication, an application typically maintains a session or equivalent token-based context so the user does not need to sign in for every action. The details may involve a server-side session, cookies, access tokens or a combination. A cookie is a storage and transport mechanism, not by itself a complete session design.

For Klyvero, the product questions are concrete: how long can someone stay signed in, what happens when an administrator removes them, and what does “log out everywhere” actually promise?

## Specify the lifecycle

```text
Sign in
  -> active session
  -> expires, is revoked, or user logs out
  -> protected action requires authentication again
```

This is a conceptual lifecycle, not an implementation protocol. Background requests, token refresh and multiple devices can make the actual behavior more complex.

| Decision | Meaning | Product question |
|---|---|---|
| Idle timeout | Session expires after a defined period without qualifying activity | Does background activity keep it alive? |
| Absolute lifetime | Session has a maximum age under the chosen policy | When must an actively working user authenticate again? |
| Reauthentication | Fresh proof is required for a sensitive action | Which actions need it, and how is unfinished work preserved? |
| Current-device logout | Ends access in the relevant local context | Does another browser or device remain signed in? |
| Global revocation | Attempts to invalidate the user's broader active access | What delay and coverage does the architecture support? |

There is no universal timeout appropriate for every product. Security should assess threats, user needs and the organization's requirements.

## Worked scenario: a removed workspace member

A contractor is signed into Klyvero on a laptop and phone. An administrator removes access to Workspace A while the contractor still belongs to Workspace B.

The desired result may be “cannot read or change Workspace A anymore, but can continue in Workspace B.” Logging out the entire account is one possible policy, not the only model. Conversely, hiding Workspace A in the interface is insufficient if the server still permits its operations.

Write acceptance questions for already-open pages, new API requests, file downloads and background jobs. Previously downloaded information cannot generally be recalled by invalidating a session. In-flight operations also need a defined policy; do not assume a permissions change retroactively cancels work already completed.

## Logout is not necessarily instant revocation everywhere

Deleting a browser cookie removes that browser's copy, but may not invalidate a stolen or separately held credential. Some server-side sessions can be centrally invalidated; self-contained access tokens may remain usable until expiry unless the system checks revocation or another control. Refresh-token revocation and access-token revocation can have different effects.

Ask Engineering and Security to explain the actual enforcement points and maximum delay. Then make UI promises match those guarantees. “Signed out on this device” and “all active access revoked” are different claims.

## Avoid common mistakes

Session expiry should not silently destroy unsaved work. Recovery of a draft still needs access control; preserving usability does not justify exposing another user's data. A password change also should not be assumed to revoke every session automatically—confirm the policy and implementation.

Support teams should not request session cookies or tokens to diagnose login problems. Use approved, non-secret diagnostic references. Session identifiers are credentials when possession grants access.

See [authentication vs authorization](authentication-vs-authorization.md), [API keys vs OAuth](api-keys-vs-oauth-for-product-managers.md), and [MFA and recovery](mfa-and-account-recovery-for-product-managers.md) for the surrounding access lifecycle.

## Security reference

[OWASP: Session Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html) covers session lifecycle and security controls. Use it with Security, not as a substitute for an implementation review.

## Related resources

- [SSO and Employee Lifecycle for Product Managers](sso-and-employee-lifecycle-for-product-managers.md) — Separate sign-in federation from provisioning, role changes and deprovisioning.

---

### Design access beyond the login screen

**Security for Product Managers** — explore Klyvero’s security decisions and the product implications of identity, permissions and session behavior.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/security-for-product-managers/)
