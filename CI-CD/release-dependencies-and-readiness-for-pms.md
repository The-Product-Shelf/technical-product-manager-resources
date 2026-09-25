# Release Dependencies and Readiness for PMs

## Replace “everything is done” with evidence of compatibility

Lumen's offline lesson update depends on a backend response, a mobile client, a content format and support guidance. Each team can finish its task while the combined journey remains unready. A release-readiness review should expose those dependencies and the evidence needed to coordinate them.

This reference complements [progressive rollouts](progressive-rollouts-for-product-managers.md), which focuses on exposure gates. Here the question is whether the required pieces can work together and recover safely.

## A compact dependency record

| Dependency | Readiness evidence | Owner's decision |
|---|---|---|
| Backend contract | Old and new supported clients behave correctly | Which client capabilities may receive the new response? |
| Mobile artifact | Approved/available state and supported-version evidence | When can eligible devices obtain and use it? |
| Content format | Representative online and offline lessons load correctly | When can new-format content be generated? |
| Data transition | Compatibility and recovery constraints documented | Which writes prevent a simple rollback? |
| Observability | Outcome metrics, failure signals and investigation route | Who detects and responds to customer impact? |
| Support and communication | Known limits and customer actions reviewed | Who communicates what, and when? |

Use named owners and links to evidence in the team's actual record. A green status without a stated acceptance condition cannot distinguish “code merged” from “ready for customer exposure.”

## Worked sequence: an offline-format change

One possible sequence is to deploy a backend that still serves the old format, distribute clients that understand both formats, verify eligible-version adoption, then enable the new format only where compatible. The correct order depends on the actual contract; this is not a universal deployment recipe.

```text
Compatible backend and content path
  -> validated client availability
  -> eligibility and recovery checks
  -> controlled exposure
  -> evidence review and wider use
```

Do not count app-store availability as installation. Offline devices may remain on older versions or retain older content. A feature flag helps only when the participating components implement the necessary controls. See [mobile releases](mobile-app-releases-for-product-managers.md).

## Review recovery before the irreversible step

If new content is written in a format the old client cannot read, reverting a server version may not restore the experience. Identify whether recovery means disabling new exposure, serving an older representation, repairing data or shipping a new client. Engineering should validate the feasible paths and their limits.

Record the decision-maker, stop condition, customer consequence and evidence needed to resume. An unresolved high-impact dependency should not disappear inside an overall percentage-complete estimate.

## Distinguish a gate from an update

A status update reports progress. A gate is a decision based on specified evidence. Keep the gate small enough to evaluate: compatibility demonstrated, monitoring operational, ownership confirmed and known risks accepted by the appropriate people. Avoid a long checklist of unrelated approvals that does not change the release decision.

If an owner is unavailable during the planned window, resolve coverage before exposure. If a dependency slips, clarify whether the product can launch a smaller coherent capability or must wait; do not expose a broken partial promise merely because one component is ready.

## A useful readiness conclusion

“Backend and client compatibility are verified for the eligible versions; offline recovery remains untested, so exposure is held until that scenario is checked” is actionable. “Ninety-five percent ready” is not.

Use [environment parity](environment-parity-for-product-managers.md) to qualify test evidence and [deployment/release/rollback](deployment-release-rollback-feature-flags.md) to keep milestone language consistent.

## Primary reference

[Google SRE: release engineering](https://sre.google/sre-book/release-engineering/) discusses controlled, reproducible delivery. The cross-team record above translates those concerns into product-readiness questions.

---

### Coordinate the path from code to customer use

**CI/CD and releases for Product Managers** — explore Lumen’s release decisions and understand why deployment, compatibility, exposure and recovery need separate evidence.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/ci-cd-and-releases-for-pms/)
