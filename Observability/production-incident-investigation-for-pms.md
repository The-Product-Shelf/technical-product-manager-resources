# Production Incident Investigation for PMs

## The PM's useful contribution

During a production incident, a PM can clarify customer impact, preserve evidence and support communication while the designated incident lead coordinates the response. Follow the team's incident process and access rules. Investigation is not permission to change production settings, replay payments or run unreviewed queries.

For Velora, “checkout errors increased” is a starting signal. The product question is whether people can place food orders, whether any payments have uncertain outcomes, and which audiences or restaurants are affected.

## A first-pass investigation card

| Field | Useful entry | Avoid |
|---|---|---|
| Observed behavior | Checkout confirmation is timing out | “The database is broken” without evidence |
| First known time | Timestamp, timezone and source | Treating first report as exact incident start |
| Population | Region, platform, version, restaurant | Assuming every user is affected |
| Impact measure | Affected unique checkouts / eligible attempts, with window | Counting log lines as customers |
| Known changes | Recent release or dependency change | Assuming timing proves causation |
| Owner and next update | Named response role and update time | Unowned investigation threads |

Record uncertainty directly. “At least 12 distinct checkout references reported between 10:00 and 10:15 UTC; total impact not yet known” is more informative than an unsupported percentage.

## Separate evidence from hypotheses

Suppose errors increase after a payment configuration change. That creates a hypothesis, not proof. Compare affected and unaffected paths, ask Engineering to check the relevant dependency, and consider traffic or instrumentation changes.

Use an evidence log with three columns: observation, interpretation, next check. A timeout is an observation; “the payment failed” is an interpretation that needs validation. [Logs and correlation IDs](reading-logs-and-correlation-ids-for-pms.md) can connect attempts, but the authoritative order and payment state matters when deciding customer follow-up.

## Prioritize decisions that reduce uncertainty or harm

1. Escalate through the established incident channel and identify the response lead.
2. Clarify whether the issue is ongoing and whether a sensitive outcome—such as duplicate charging—needs specialist handling.
3. Share a compact evidence packet: timestamps, redacted references, reproduction conditions and links to approved dashboards.
4. Help the response lead compare customer consequences of mitigation options.
5. Keep support and customer messaging aligned with verified facts and the agreed communication owner.

A rollback, flag change or traffic shift may help, but each has prerequisites. Disabling checkout could prevent new uncertain outcomes while also preventing valid purchases. Reverting code may not reverse data changes or external transactions. The technical owner should assess feasibility and execute the agreed mitigation.

## A useful internal update

```text
Observed: some mobile checkouts time out before confirmation.
Scope: reports currently cluster on one payment path; total impact unknown.
Risk: a timeout may still correspond to a completed payment.
Action: incident lead is checking payment and order states.
Next update: 10:30 UTC, or sooner if impact or mitigation changes.
```

Do not include confidential customer records or credentials in a shared incident note. Use the team's approved references and communication channels.

## Recovery needs evidence too

A falling error rate is encouraging, but verify the intended customer journey across affected segments. Check pending jobs, delayed confirmations and unresolved transactions. Confirm whether monitoring recovered because users succeeded or because traffic disappeared.

Distinguish mitigation from full resolution. Capture follow-up owners for reconciliation, customer remediation, monitoring gaps and a blameless review. Preserve the original timeline and corrected assumptions so the team can learn from the response.

Related: [percentiles](percentiles-p95-and-p99-for-pms.md) for performance interpretation and [deployment, release and rollback](../CI-CD/deployment-release-rollback-feature-flags.md) for mitigation vocabulary.

## Operational reference

[Google SRE: incident response](https://sre.google/workbook/incident-response/) describes coordinated roles, communication and learning from incidents.

## Related resources

- [Actionable Dashboards and Alerts for PMs](actionable-dashboards-and-alerts-for-pms.md) — Connect a signal to a customer outcome, response owner and useful next action.

---

### Connect incident signals with customer impact

**Logs, Kibana, and Observability for Product Managers** — explore Velora’s investigation scenarios and use observability evidence to support clearer product decisions.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/logs-kibana-observability-product-managers/)
