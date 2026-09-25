# Automated Tests for Product Managers

## Ask what risk a test helps detect

Automated tests run repeatable checks against expected behavior. Different test types provide different evidence; passing a suite does not prove the product is defect-free or useful. Teams use test labels differently, so ask what the test actually includes rather than relying only on its name.

For Lumen, a lesson-completion change can affect progress calculation, stored data, mobile synchronization and the visible learning journey. One test layer is unlikely to cover all of those risks.

## Reference by product question

| Test type | Plain-English meaning | Why a PM should care |
|---|---|---|
| Unit | Checks a small piece of behavior in isolation | Can catch a rule error quickly, such as an incorrect streak calculation |
| Integration | Checks collaborating components | Can reveal whether progress is stored and read correctly |
| Contract | Checks agreed interaction expectations between components | Helps catch incompatible API or event changes |
| End-to-end | Exercises a broader journey through an assembled system | Can show whether a learner can finish a lesson and see progress |
| Regression | Rechecks behavior that should keep working | Protects existing use cases; it describes a purpose, not a separate architecture layer |
| Smoke | A small set of checks for basic readiness | Can quickly reveal that a deployment cannot serve a core path |

A contract test does not establish every aspect of business correctness. An end-to-end test also depends on which systems, environments and external services are real versus simulated.

## Worked example: a streak around midnight

Lumen promises that a completed lesson counts toward the learner's local calendar day. A learner finishes near midnight and the device synchronizes later.

Useful acceptance examples include:

- Completion before local midnight is assigned to the intended day under the agreed event-time rule.
- Repeated delivery of the same completion does not add another lesson or streak day.
- The server and client agree on the stored result after synchronization.
- An older supported app version still displays the progress correctly.
- A timezone change follows an explicit policy rather than accidentally duplicating a day.

These examples describe product behavior. Engineering can decide which should be unit, integration, contract or journey tests and which need additional exploratory testing. The exact timezone policy must come from the product's promise, not an assumption hidden in test code.

## Read a test result with its limits

A failing test may indicate a real regression, an environment issue or a faulty test. A flaky test gives inconsistent results under supposedly equivalent conditions; repeatedly rerunning until green does not resolve the uncertainty.

A passing test means its implemented assertions passed in that run. It does not guarantee the assertions are complete, production data has the same shape, or every supported device works. Mocked payment or identity services also may not reproduce every real dependency behavior.

Code coverage measures code exercised under a particular definition. High coverage can coexist with weak assertions and missing customer scenarios. Treat it as one engineering signal rather than a product quality score.

## Questions for release readiness

Which customer promises are covered? What changed that existing checks might miss? Are failures understood? Which dependencies are simulated? What remaining risk needs manual exploration, monitoring or a smaller rollout? Who decides whether that risk is acceptable?

Use the [CI/CD glossary](cicd-glossary-for-product-managers.md) to locate tests in the delivery process and [progressive rollouts](progressive-rollouts-for-product-managers.md) to discuss evidence after deployment. Tests and production observation complement each other.

## Technical reference

[Martin Fowler: The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html) discusses test layers and trade-offs. Its guidance is not a requirement to adopt a fixed test-count ratio.

## Related resources

- [Environment Parity for Product Managers](environment-parity-for-product-managers.md) — Explain what staging evidence transfers to production and which differences remain.

---

### Connect delivery checks with product confidence

**CI/CD and releases for Product Managers** — explore Lumen’s CI/CD journey and understand what validation can establish before customers encounter a change.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/ci-cd-and-releases-for-pms/)
