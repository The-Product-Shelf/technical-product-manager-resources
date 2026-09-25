# Security Glossary for Product Managers

A vocabulary for discussing who can do what, which data is exposed, and what could go wrong.

Klyvero is a fictional project management product. A customer asks for an “Export users” button. That request introduces decisions about permissions, downloaded data, access duration and accountability before any security tool is selected.

## Understand the risk

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Asset** | Something worth protecting: data, money, access or a critical capability. | Name what an abuse case could harm. |
| **Threat** | A potential source or cause of harm. | Consider an unauthorized person trying to obtain an organization's user list. |
| **Vulnerability** | A weakness that could be exploited. | An export endpoint that omits an organization check could expose another customer's data. |
| **Risk** | The possibility of harm, considered through likelihood and impact. | Prioritization needs context, not just the name of a weakness. |
| **Attack surface** | The exposed paths through which a system could be targeted. | New integrations, exports and recovery flows can create new paths to review. |
| **Least privilege** | Granting only the access needed for the task. | An integration that reads tasks may not need permission to delete them. |

## Identity and access

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Authentication** | Establishing identity. | Define how users prove who they are. |
| **Authorization** | Deciding whether an action on a resource is permitted. | A valid login does not grant access to every project. |
| **Session** | The context that connects later interactions to an authenticated user. | Define logout, expiry and what happens after an account is deactivated. |
| **MFA** | Authentication using factors from different categories. | Two passwords are not two factors; recovery must also be considered. |
| **SSO** | Using a shared identity system to sign in to connected applications. | Sign-in and employee departure flows may depend on the customer's identity provider. |
| **OAuth** | A framework for delegated authorization. | Understand which access an integration requests and how it is revoked. |
| **Scope** | A named boundary of access requested or granted in a system. | Explain integration permissions in terms users can understand. |
| **Secret** | Sensitive information whose possession enables access or trust, such as an API key. | Do not put keys in support screenshots, examples or exported records. |

See OWASP's [authentication guidance](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html) and the [OAuth framework](https://www.rfc-editor.org/rfc/rfc6749.html). OAuth alone should not be treated as a login protocol; identity flows can use a layer such as OpenID Connect.

## Data protection

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Encryption** | Transforming data so the appropriate key is needed to recover it. | Ask about protection in transit and storage, and who can access decrypted data. |
| **Hashing** | Producing a digest from input; it is not intended to be reversed to recover that input. | Password storage requires suitable password-hashing methods, not merely “some hash.” |
| **PII** | Information that can identify a person directly or in combination. | Treat exports and telemetry as potential sources of identifiable information. |
| **Data minimization** | Collecting and keeping only the data needed for a defined purpose. | An export may not need every field available in the database. |
| **Audit log** | A record of relevant actions intended to support accountability. | Decide which exports and permission changes need a record, and who can inspect it. |

For the difference between hashing and encryption in password handling, see OWASP's [password storage guidance](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html).

## Example: define the export before building it

A proposed Klyvero policy might let designated administrators export members of their own organization, while excluding unnecessary profile fields. The downloadable file would have a defined access lifetime, and the system would record who requested it.

Those are product requirements to review with Engineering and Security. Encryption would not fix an authorization rule that allows the wrong administrator to download the file. Likewise, removing access to the hosted download cannot retract a copy that someone has already saved.

Clarify the intended behavior when the requester loses their role before the export completes. “Administrator only” is insufficient without defining the organization, resource and time at which access is checked.

## Questions worth asking Engineering and Security

- What are we protecting, and which abuse scenarios matter here?
- Who needs this capability, for which resources and for how long?
- Which fields can we omit from the export or logs?
- What happens when membership, roles or credentials change?
- What evidence would help investigate misuse without collecting excessive data?

For a worked permissions example, see [Authentication vs Authorization](authentication-vs-authorization.md).

---

### Apply security concepts to everyday features

This resource is part of The Product Shelf's free Technical Product Management library.

**Security for Product Managers**

Explore Klyvero’s sign-in, session, integration and data-protection decisions from the perspective of product behavior and risk.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/security-for-product-managers/)
