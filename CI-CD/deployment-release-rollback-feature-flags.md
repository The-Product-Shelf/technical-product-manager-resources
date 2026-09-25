# Deployment vs Release vs Rollback vs Feature Flag

Separate putting software in production, making it available, and responding when something goes wrong.

Lumen is a fictional employee-learning platform. Its new Learning Streaks feature is deployed on Tuesday, but customers first see it on Thursday. Both dates can be correct: deployment and release describe different changes.

Teams use these words differently. This guide uses “release” to mean making a capability available to users; some teams also use it for a packaged version. Agree on the meaning in your own launch plan.

## The terms side by side

| Term | What changes | Lumen example | What it does not guarantee |
| --- | --- | --- | --- |
| **Deployment** | A software version is installed or updated in an environment. | The new backend version reaches production on Tuesday. | Customers can see or use Learning Streaks. |
| **Release** | A capability becomes available to an intended audience. | Streaks is enabled for selected customer accounts on Thursday. | Everyone receives it at the same time. |
| **Feature flag** | A condition controls whether a behavior is enabled. | Streaks is enabled for internal accounts first. | All related background behavior is disabled when the visible feature is off. |
| **Rollout** | Exposure expands across an audience over time. | More eligible accounts receive Streaks after review. | A percentage alone makes the launch safe. |
| **Rollback** | An earlier software version is restored. | The team replaces version B with version A. | Data changes or external effects are undone. |
| **Roll forward** | A new version fixes a problem. | Version C corrects the defect in B. | The impact is contained while the fix is prepared. |
| **Hotfix** | An urgent fix follows an expedited process. | A serious streak-calculation bug receives a priority correction. | Validation is unnecessary. |

For more on separating feature exposure from deployment, see [Feature Toggles](https://martinfowler.com/articles/feature-toggles.html).

```text
Deploy version B to production
Learning Streaks remains OFF for customers
                    |
          Enable flag for a group
                    v
Version B: Streaks ON for that group
                    |
              Disable flag
                    v
Version B: Streaks OFF again

Version recovery is a separate action:
Rollback:      B --> A (earlier version)
Roll forward:  B --> C (version with a fix)
```

Turning off the flag leaves version B deployed. Whether it stops the faulty behavior depends on what the flag controls; changing versions does not automatically undo changes to data.

## Example: expand only after reviewing the evidence

An illustrative Lumen launch could proceed as follows:

1. Deploy the change with customer exposure disabled.
2. Enable it for internal accounts and verify expected behavior.
3. Enable it for a small, defined group of eligible customer accounts.
4. Review calculation correctness, failures and support feedback over an agreed window.
5. Expand exposure if the agreed conditions are met.

These steps are not a universal rollout policy. Choose the audience, sample size, review window and stopping conditions based on the feature and its risk.

For an account-based product, exposing 5% of accounts is different from exposing 5% of users or requests. Whole-account exposure might preserve a consistent experience for colleagues. Also ask whether selected accounts represent the situations we need to test: a quiet account may never exercise a timezone boundary or a high-volume workflow.

## Three actions during a problem

Suppose some learners receive an incorrect streak after midnight.

**Pause the rollout.** Stop expanding exposure. Existing recipients may still experience the problem, so this alone may not contain the impact.

**Disable the flag.** Turn off the behavior it actually controls. If the flag only hides a badge, an incorrect background calculation could continue. Verify the flag's scope before relying on it for recovery.

**Roll back or fix forward.** Engineering evaluates version compatibility, data changes and the available fix. Product contributes the affected users, severity and consequences of leaving the problem active.

None of these actions automatically repairs incorrect streak records or retracts a notification already sent. Recovery may require separate data repair and communication work.

## Define recovery before launch

Agree on who can stop exposure, what evidence they need, how quickly the action takes effect, and how the team confirms that the harmful behavior has stopped. Record remaining work after containment rather than treating “flag off” as the end of the incident.

Flags also need an owner and a removal plan when they are no longer needed. Old alternatives increase the number of states the team must understand and test.

For mobile apps, remember that an already-installed version can remain on users' devices. A remotely controlled flag may help only if the installed code consults it and the faulty behavior is covered. A server rollback does not replace every installed app.

## Questions worth asking Engineering

- What is deployed, what is enabled, and for which audience?
- What exactly does the flag control, including background work?
- Which signals and conditions govern expansion or stopping?
- Is rollback compatible with current data and dependencies?
- After containment, what data, notifications or customer issues still need repair?

For the earlier stages of delivery, use the [CI/CD glossary](cicd-glossary-for-product-managers.md).

---

### Explore launch and recovery decisions

This resource is part of The Product Shelf's free Technical Product Management library.

**CI/CD and releases for Product Managers**

Use Lumen’s Learning Streaks scenarios to examine progressive exposure, rollback, fixes and what changes when a release reaches mobile devices.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/ci-cd-and-releases-for-pms/)
