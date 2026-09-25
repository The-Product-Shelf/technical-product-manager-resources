# Mobile App Releases for Product Managers

## There are several release moments

For Lumen's mobile learning app, uploading a build does not mean customers have installed it. Store processing, review, distribution, device updates and feature exposure are separate steps, and policies differ by platform and distribution route.

```text
Build and validate
  -> submit to the store
  -> review and release availability
  -> customer installs or updates
  -> eligible customer sees the feature
```

A remotely controlled feature may remain off after installation. Some features cannot be controlled remotely, and platform policies still apply. Plan according to the actual implementation.

## Track the states separately

| State | What it establishes | What it does not establish |
|---|---|---|
| Build ready | A specific artifact exists and has passed agreed checks | Store approval |
| Approved | Store review requirements were satisfied for that submission | Every customer has the version |
| Available | Eligible users can obtain the release under store settings | Installation across the population |
| Installed | A device is running that version | New behavior is enabled or successfully used |
| Exposed | An eligible audience can access the behavior | A successful customer outcome |

Avoid a single ambiguous “released” date when coordinating support, marketing and engineering. Label which event each milestone represents.

## Worked scenario: a new offline lesson format

Lumen's backend begins producing an updated offline lesson format. The new mobile version understands it, but older installed versions may remain active for weeks or longer.

Before enabling the format, ask how the server identifies client capability, what older clients receive, and whether cached lessons still open. A feature flag can limit exposure only if the relevant clients and backend implement the needed controls. It cannot retroactively teach an old binary to understand a new format.

The team may need a period of backward compatibility, an agreed minimum supported version and a customer-friendly update path. A forced update can block learning for people unable to install the new version, so evaluate device support, connectivity and accessibility consequences.

## Store rollout is not the same as a product cohort

Apple's phased release for eligible version updates gradually controls automatic updates; users can still manually download the update. Google Play staged rollout controls availability to a proportion of users under its rules. Neither should be assumed to match a product's stable workspace or learner assignment.

Use current store documentation when planning a release because eligibility, timing and controls can change. Keep product-side exposure and installed-version measurement distinct from store rollout settings.

## Recovery differs from a server deployment

Halting a store rollout can stop further distribution under the platform's rules, but does not remove the version from devices that already installed it. An affected customer may need a later fixed build, potentially involving another store review and installation delay.

A backend mitigation or an existing feature flag may reduce impact while a fix is distributed. Confirm that the mitigation works with both old and new clients and with offline behavior. Reverting backend behavior can itself break clients that now rely on the new contract.

## A PM's readiness checklist

- Which versions and devices are eligible and supported?
- What remains compatible while versions coexist?
- Can the team measure crashes, failures and learning completion by version?
- What can be disabled remotely, and how quickly do offline clients observe it?
- Who owns store status, support guidance and the recovery decision?

Use [progressive rollouts](progressive-rollouts-for-product-managers.md) for exposure planning and [automated tests](automated-tests-for-product-managers.md) for compatibility scenarios.

## Platform references

- [Apple: release a version update in phases](https://developer.apple.com/help/app-store-connect/update-your-app/release-a-version-update-in-phases/).
- [Google Play: staged rollouts](https://support.google.com/googleplay/android-developer/answer/6346149).

---

### Coordinate deployment with the customer’s actual version

**CI/CD and releases for Product Managers** — follow Lumen’s release scenarios and connect delivery mechanics with exposure, validation and recovery.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/ci-cd-and-releases-for-pms/)
