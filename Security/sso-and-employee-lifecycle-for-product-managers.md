# SSO and Employee Lifecycle for Product Managers

## SSO does not complete the access lifecycle

A customer asks Klyvero for single sign-on (SSO). Centralizing sign-in can improve administration and the employee experience, but it does not by itself create the correct memberships, update permissions or immediately revoke every active session when someone leaves.

Treat the requirement as a joiner, mover and leaver journey. Product identifies expected behavior and failure scenarios; Security and Engineering assess protocols, identity binding and enforcement.

## Separate the responsibilities

| Responsibility | What it does | What to verify |
|---|---|---|
| Sign-in federation | Uses an identity provider to establish identity for the application | Which organization and identity are being authenticated? |
| Provisioning | Creates or updates the application's account/membership records | When does access appear, and which defaults apply? |
| Authorization mapping | Maps approved attributes or groups into permissions | What happens when group membership changes or conflicts? |
| Deprovisioning | Removes or disables access under a defined policy | Which memberships, sessions and credentials are covered? |
| Recovery | Restores legitimate administration during failures | Who may use the reviewed recovery route? |

OpenID Connect and SAML are commonly used for federation. SCIM is a protocol for managing identity resources and can support provisioning workflows; its presence does not establish instant end-to-end deprovisioning. Confirm the actual integration's supported operations and delays.

## Worked scenario: a contractor changes teams

Laura uses the customer's identity provider to access Klyvero. She moves from Team A to Team B. The identity provider updates her groups, but Klyvero has an active session and cached membership data.

Define when Team A access must stop, when Team B access begins, and what happens to already-running exports. Existing [session controls](sessions-for-product-managers.md) and authorization checks determine enforcement; a successful new login is only one part of the lifecycle.

Do not automatically merge accounts just because email addresses match. Addresses can change, be reused or belong to different identity contexts. Engineering and Security should establish stable identity binding and a reviewed account-linking process.

## Turn “employee removed” into acceptance scenarios

```text
Identity source changes
  -> provisioning or application policy processes the change
  -> memberships and permissions are updated
  -> active access is restricted under the agreed revocation policy
  -> evidence confirms the intended result
```

Test removal while signed in on multiple devices, loss of a privileged group, a delayed provisioning message and rehire after departure. Also inventory access outside the human session: a separately owned integration or service credential may not follow the employee's login lifecycle.

Disabling access and deleting business records are separate actions. Decide how owned projects are reassigned and how audit evidence is retained under the approved data policy. A departure should not unexpectedly erase teammates' work.

## Plan for identity-provider failure

If the provider is unavailable, decide which existing sessions may continue and how administrators recover. Do not prescribe a weaker fallback solely to reduce login friction. Any exceptional route needs Security review, controlled ownership and an auditable process.

Ask whether the organization's MFA requirements are actually enforced for this application, including reauthentication and recovery paths. SSO does not automatically give every session the same assurance.

Related: [authentication and authorization](authentication-vs-authorization.md), [MFA and recovery](mfa-and-account-recovery-for-product-managers.md) and [data minimization and audit logs](data-minimization-and-audit-logs-for-pms.md).

## Primary references

[OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html) describes federated authentication and claims. [SCIM RFC 7644](https://www.rfc-editor.org/rfc/rfc7644.html) describes identity-resource management operations. Product promises must reflect the implemented lifecycle across both sides.

---

### Follow access from arrival to departure

**Security for Product Managers** — explore Klyvero’s identity and permission scenarios to define clear behavior when people, roles and organizations change.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/security-for-product-managers/)
