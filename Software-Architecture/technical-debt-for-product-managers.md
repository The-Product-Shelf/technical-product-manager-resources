# Technical Debt for Product Managers

How do you evaluate a technical-debt request without either dismissing it or treating the label as automatic priority?

Nómada is a fictional online shop. Engineering says changes to checkout take longer because pricing rules are duplicated. The useful conversation is about a specific constraint, its consequences and the options for reducing it.

## Turn the label into a decision

Technical debt describes future costs or constraints arising from design or implementation choices. Not every defect, maintenance task or old technology is automatically debt. The metaphor is useful when it clarifies a trade-off; it is less useful when it replaces an explanation. See Martin Fowler's [technical debt discussion](https://martinfowler.com/bliki/TechnicalDebt.html).

A concise proposal can answer these questions:

| Question | Example evidence to seek |
| --- | --- |
| Where is the constraint? | Pricing logic duplicated across checkout paths. |
| What does it cost now? | Repeated changes and defects traced to inconsistent rules. |
| Which future work encounters it? | An upcoming promotion or currency change. |
| What are the options? | Local fix, clearer shared boundary, larger redesign, or defer. |
| What will improvement look like? | A change applied consistently through fewer well-defined paths. |
| How will the transition be checked? | Comparison of prices and critical purchase outcomes before and after. |

The example is hypothetical. Record actual observations rather than attaching invented savings to a refactor.

## Separate the problem from the preferred solution

“The checkout is hard to change” does not by itself establish that Nómada needs microservices. The cause might be duplicated logic, poor tests, unclear ownership or tightly coupled modules.

Ask what smaller intervention could reduce the constraint and what the larger proposal enables beyond it. A technically attractive redesign can still have migration costs and new operating demands. Conversely, repeatedly patching a known constraint can be more expensive than addressing its source.

The [monolith vs microservices guide](monolith-vs-microservices-for-pms.md) helps distinguish structural options from a presumed maturity ladder.

## Example: promotions are coming next quarter

Suppose Nómada expects to add two new promotion types. Engineering can show that each currently requires edits in several places and that previous pricing defects came from inconsistent updates.

Product can help establish the business consequences: which customers or orders are affected, how the constraint intersects with committed work, and how much uncertainty exists. Engineering can assess the implementation choices and whether the proposed change actually removes the duplication.

A staged plan might first isolate one pricing rule and compare results on representative cases. The team could then decide whether to extend the approach. This is a way to gather evidence, not a requirement that every architectural change fit a small experiment.

## Avoid false precision

A score can help compare proposals, but “saves 40% of development time” needs evidence and a meaningful baseline. Delivery time can change for many reasons. Track leading evidence such as reduced duplication or clearer ownership alongside later outcomes such as fewer related defects.

Also include the cost of transition: temporary parallel paths, compatibility, data migration, test updates and rollback limitations. A project is not complete merely because the new code exists; the old constraint must actually be retired where intended.

## Questions for a prioritization conversation

- What observation would convince us this work is urgent, or safe to defer?
- Which planned product change will run into the constraint?
- What is the smallest useful outcome, and what remains afterward?
- How will users be protected while behavior moves between implementations?
- When will we review whether the expected benefit occurred?

Use [automated tests](../CI-CD/automated-tests-for-product-managers.md) to discuss evidence for behavior preservation and [progressive rollouts](../CI-CD/progressive-rollouts-for-product-managers.md) when exposure can be controlled.

## Related resources

- [Scalability for Product Managers](scalability-for-product-managers.md) — Translate a campaign forecast into workload assumptions and capacity evidence.

---

### Bring architectural constraints into product planning

**Software Architecture for Product Managers** — explore the problems that lead Nómada to change its architecture and the trade-offs that accompany those changes.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/software-architecture-for-pms/)
