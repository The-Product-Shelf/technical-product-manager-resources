# MFA and Account Recovery for Product Managers

## Protect the account without losing the person

Multi-factor authentication (MFA) requires evidence from more than one factor category, such as knowledge, possession or inherence. A password and a PIN are both knowledge factors; two prompts are not automatically two factors. The security properties of a method matter as much as the label “MFA.”

For Klyvero, an administrator can export workspace data and manage membership. Protecting that access matters, but an administrator who loses a phone still needs a legitimate recovery path. Recovery is part of the security design, not an exception that can ignore it.

## Compare methods by the problem they solve

| Method or control | Useful property | Product consideration |
|---|---|---|
| Authenticator-generated code | Adds possession-based evidence alongside another factor | Codes can still be phished; device loss needs recovery |
| Push approval | Can make approval convenient | Unexpected prompts and approval fatigue need attention |
| SMS code | Familiar and widely reachable in some markets | Phone-number takeover and delivery limitations affect risk and usability |
| FIDO/WebAuthn-based method | Can provide phishing resistance through origin binding | Device support, enrollment and recovery still matter |
| Recovery code | Can restore access under a defined policy | Treat it as a secret; it should not be casually shared with support |

Not every passkey interaction should be described identically: whether a deployment meets a particular multi-factor assurance requirement depends on its configuration and user verification. Have Security assess the actual method and requirements.

## Worked scenario: the sole administrator loses a device

Klyvero's only workspace administrator loses the phone used for authentication. Automatically disabling MFA after an email request would give an attacker a route around the normal controls. Permanently locking the organization out is also a serious product failure.

Before launch, define the available recovery paths: previously enrolled alternative methods, securely stored recovery codes, or an approved support-assisted process with appropriate evidence and review. Organization policy may restrict which options are allowed. Avoid designing identity checks from ad hoc facts that support staff can easily obtain from public sources.

## Specify the sensitive transitions

```text
Enroll method
  -> verify it works
  -> normal sign-in and sensitive actions
  -> device lost or method replaced
  -> approved recovery and re-enrollment
```

Changing an MFA method can be as sensitive as signing in. Decide when fresh authentication is required, which notifications go to existing channels, and whether other sessions should be revoked. A recovery flow should consider compromised access as well as simple device loss.

Show customers clear next steps without exposing secret values or internal verification details. Provide accessible alternatives that have been assessed for the relevant risk; an inaccessible enrollment flow can block legitimate use before the product provides value.

## Measure both security and completion

Track enrollment completion, challenge failures, recovery volume and time to recover. Pair those with abuse indicators and support findings. Lower recovery time is not automatically better if a shortcut weakens verification; fewer support requests can also mean users abandoned the process.

Use segmented, access-controlled analysis. Do not log authentication secrets or ask users to paste recovery codes into tickets. Codes and tokens should be handled only through the intended verification flow.

## Questions for Security and Support

Which accounts and actions require stronger assurance? What happens when every enrolled method is unavailable? Who may approve recovery, and what evidence is required? How are suspicious changes communicated? Can the team rehearse device-loss and compromised-account scenarios before rollout?

Read [sessions](sessions-for-product-managers.md) for expiry and revocation, and [authentication vs authorization](authentication-vs-authorization.md) for the boundaries between proving identity and granting permissions.

## Security reference

[OWASP: Multifactor Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Multifactor_Authentication_Cheat_Sheet.html) covers factor choices, recovery and factor replacement risks.

---

### Design security as a complete user journey

**Security for Product Managers** — explore Klyvero’s security trade-offs across account access, permissions and practical product decisions.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/security-for-product-managers/)
