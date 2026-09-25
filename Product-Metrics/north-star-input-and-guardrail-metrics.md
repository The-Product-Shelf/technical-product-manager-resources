# North Star, Input Metrics & Guardrails

How do we connect a product goal to things a team can influence without rewarding harmful shortcuts?

Klyvero is a fictional project management product. Suppose its strategy centers on helping teams make progress together. More signups alone would not tell us whether that is happening.

A useful measurement system connects an outcome, possible drivers and conditions we want to protect. The model below is an illustrative proposal to validate with research and data, not a claim about what every collaboration product should measure.

## Three different jobs

| Metric type | Its role | Illustrative Klyvero example |
| --- | --- | --- |
| **North Star** | A central measure intended to reflect value delivered and progress toward the strategy. | Weekly accounts in which at least two members complete tasks. |
| **Input metric** | A more directly influenceable behavior or condition believed to contribute to that outcome. | New accounts with a second contributing member within seven days. |
| **Guardrail** | A measure that exposes unacceptable side effects while pursuing the goal. | Reports of unwanted invitations per 1,000 invitations delivered. |

The [Amplitude North Star framework](https://amplitude.com/books/north-star) describes the relationship between a North Star and its inputs. A selected metric remains a hypothesis about value, not a substitute for understanding customers.

## Start with the value claim

Why might two members completing tasks indicate value? It suggests participation beyond one person setting up an empty workspace. But the behavior could also be superficial: users might split trivial work into many tasks or complete tasks they did not meaningfully finish.

Before adopting this metric, investigate whether it corresponds to useful collaboration and continued use. Define what counts as a completed task, how reopened tasks are handled, which accounts are eligible and how the week is bounded.

The North Star can also miss value experienced by solo users. If they are an important strategic audience, we need to revisit the metric or use complementary measures. One number cannot describe every part of a business.

## Connect inputs to an actual hypothesis

```text
More new accounts invite a relevant teammate
                    ↓
More invited teammates contribute within seven days
                    ↓
More accounts collaborate on completed work each week
```

Each arrow is a hypothesis. Increasing invitations may generate spam without increasing useful participation. Improving the invitation acceptance rate may help only if accepted invitations lead to meaningful contribution.

A team can run a focused investigation: simplify the invitation flow, then examine whether second-member contribution improves among comparable new accounts. Do not assume that every movement in an input will move the North Star.

## Make guardrails usable before the launch

For an invitation experiment, Klyvero could monitor:

| Risk | Guardrail candidate | Decision to agree in advance |
| --- | --- | --- |
| Unwanted messages | Unwanted-invitation reports per 1,000 delivered invitations | When should the team pause and investigate? |
| Technical disruption | Invitation acceptance failures ÷ acceptance attempts | Which failures require immediate action? |
| Distracting onboarding | New accounts completing their first project within seven days ÷ eligible new accounts | How much deterioration would undermine the benefit? |

Choose thresholds, observation windows and owners with Data and Engineering using the baseline, expected variation and potential harm. A tiny sample can be noisy; a severe incident may warrant action regardless of statistical precision. There is no universal “safe” percentage to copy into every launch.

## Example: invitations rise, collaboration does not

Suppose a new prompt increases invitations, but second-member contribution remains unchanged and unwanted-message reports rise. Celebrating the invitation count would miss both the intended outcome and the side effect.

The next step is to investigate who received the invitations, whether they understood the product, and what blocked participation. The team may need a better invitation experience rather than more pressure on the sender.

## Questions worth asking Product and Data

- What customer value does the North Star represent, and whom does it leave out?
- Which inputs can this team affect, and what supports the assumed relationship?
- Could the metric improve while the experience gets worse?
- Which guardrails trigger a pause, who decides, and with what evidence?
- What would make us change the metric definition itself?

For calculation details and common denominator mistakes, use the [product metrics cheat sheet](product-metrics-cheat-sheet.md).

---

### Want to go deeper?

This resource is part of The Product Shelf's free Technical Product Management library.

**Product Metrics for Product Managers** explores these topics through practical product scenarios.

→ [Explore the book at The Product Shelf](https://theproductshelf.com/product/product-metrics-for-product-managers/)
