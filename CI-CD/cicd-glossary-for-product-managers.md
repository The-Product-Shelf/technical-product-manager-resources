# CI/CD Glossary for Product Managers

Understand where a change is, what is blocking it, and what “ready” actually means.

Lumen is a fictional employee-learning platform. The team is building Learning Streaks, which tracks consecutive days of learning activity. A developer says the work is done, but customers cannot use it yet. The terms below help locate what is still pending.

## From code to a runnable version

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Repository** | A place where code and its history are managed. | Work recorded there is not necessarily available to users. |
| **Commit** | A recorded set of changes in version history. | It identifies a change, not necessarily a complete feature. |
| **Branch** | A separate line of development. | Finished work on a branch may still need integration. |
| **Pull request (PR)** | A proposal to merge changes, usually with review and checks. | “PR open” and “ready to release” are different states. |
| **Merge** | Combining changes into another branch. | Integration is a milestone, not proof of deployment. |
| **Build** | The process of producing a runnable or distributable version; teams also use the word for its output. | Ask which version was built and tested. |
| **Artifact** | An output of the build or delivery process, such as a packaged application. | The thing tested should be traceable to what is deployed. |
| **Automated test** | A programmed check of expected behavior. | Passing tests provides evidence for what they cover, not a guarantee of no defects. |

## Checks and environments

| Term | Plain-English meaning | Why it matters to a PM |
| --- | --- | --- |
| **Pipeline** | A defined sequence or graph of automated tasks and checks. | A failed step can prevent work from moving forward. |
| **Job / step** | Units of execution within a pipeline; naming varies by tool. | Knowing the failing stage helps locate the blocker. |
| **Environment** | A context in which software runs, including configuration and dependencies. | The same code can behave differently in different environments. |
| **Development** | A context used while building changes. | A local demonstration does not establish readiness for real traffic. |
| **Staging** | A pre-production environment used for validation. | Differences in data, load or configuration can limit what it proves. |
| **Production** | The environment serving real users or workloads. | Code can be present there before a feature is enabled for customers. |
| **Quality gate** | A condition that must be met before proceeding. | Understand the evidence needed, rather than assuming a failed gate should be bypassed. |

GitHub's [Actions overview](https://docs.github.com/en/actions/about-github-actions/understanding-github-actions) gives a concrete example of workflows, jobs and steps. Other tools use different names for similar ideas.

## What CI and CD mean

| Practice | Meaning | What it does not imply |
| --- | --- | --- |
| **Continuous integration (CI)** | Integrating changes frequently and checking them automatically. | Every integrated change is already in production. |
| **Continuous delivery** | Keeping changes in a releasable state through an automated delivery process; production deployment can require a decision. | Every passing change is automatically deployed. |
| **Continuous deployment** | Automatically deploying changes to production after the required checks pass. | Every feature is immediately visible to all customers. |

Because “CD” can refer to either delivery or deployment, ask which practice the team means. See Martin Fowler's [continuous delivery explanation](https://martinfowler.com/bliki/ContinuousDelivery.html).

## Example: make the status update useful

For Learning Streaks, a hypothetical update could be:

> The PR is merged. The build passed its automated checks and is running in staging. We still need to validate the timezone behavior before production deployment. Customer exposure will be controlled separately with a feature flag.

This identifies a remaining product concern: what counts as a consecutive day when a learner changes timezones? It also prevents us from telling customers that the feature is available merely because the code is merged.

A possible path is:

```text
Code changes (commit, review and checks, merge)
  |
  v
Build and validation
  |
  v
Staging
  |
  v
Production deployment (flag OFF)
  |
  v
Customer exposure (flag ON for selected audience)
```

This is a simplified example with a feature flag. Checks may run before and after merging, builds may happen at several stages, and some teams organize the path differently. Map your team's actual milestones instead of treating this sequence as universal.

## Questions worth asking Engineering

- Which milestone has this change reached, and what remains?
- Which version and environment were validated?
- What does the failed check tell us, and who is investigating it?
- Does “CD” here mean delivery or automatic deployment?
- Which conditions must be met before we communicate availability?

For launch and recovery decisions, continue with [Deployment vs Release vs Rollback vs Feature Flag](deployment-release-rollback-feature-flags.md).

## Related resources

- [Automated Tests for Product Managers](automated-tests-for-product-managers.md) — Connect test layers to product risks and remaining uncertainty.
- [Mobile App Releases for Product Managers](mobile-app-releases-for-product-managers.md) — Coordinate store distribution, installed versions and backend compatibility.

---

### Follow a feature all the way to users

This resource is part of The Product Shelf's free Technical Product Management library.

**CI/CD and releases for Product Managers**

Follow Learning Streaks through Lumen’s reviews, builds, environments and launch decisions, including the additional constraints of mobile delivery.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/ci-cd-and-releases-for-pms/)
