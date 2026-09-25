# CTEs for Product Managers

## What a CTE contributes

A common table expression, introduced with `WITH`, gives a name to a query result for use within a larger statement. It helps a reader inspect stages of an analysis. It does not automatically make a query faster, create a permanent table, or guarantee that the database stores each stage separately.

For a PM, the useful question is: **can we explain the population at each step before trusting the final percentage?**

## Worked example: adoption among eligible workspaces

Klyvero wants the share of eligible paid workspaces that used a reporting feature in January. This is workspace adoption, not user adoption or retention.

Assume `workspaces(workspace_id, eligible_for_january)` is an agreed historical eligibility snapshot, with one row per workspace. Assume `events(workspace_id, event_name, occurred_at)` contains activity. A current subscription flag would not necessarily represent January's population.

```sql
WITH eligible AS (
    SELECT workspace_id
    FROM workspaces
    WHERE eligible_for_january = TRUE
), adopters AS (
    SELECT DISTINCT workspace_id
    FROM events
    WHERE event_name = 'report_exported'
      AND occurred_at >= '2026-01-01 00:00:00'
      AND occurred_at <  '2026-02-01 00:00:00'
), counts AS (
    SELECT
        COUNT(*) AS eligible_workspaces,
        COUNT(a.workspace_id) AS adopting_workspaces
    FROM eligible e
    LEFT JOIN adopters a
      ON e.workspace_id = a.workspace_id
)
SELECT
    eligible_workspaces,
    adopting_workspaces,
    100.0 * adopting_workspaces
        / NULLIF(eligible_workspaces, 0) AS adoption_pct
FROM counts;
```

This PostgreSQL-compatible example uses `100.0` to avoid integer division and `NULLIF` to return NULL when there is no eligible population. “No eligible workspaces” is different from “eligible workspaces, none adopted.” Agree reporting timezone before using the date boundaries.

## Hand-check the stages

Suppose eligible workspaces are alpha, beta and gamma. Alpha exports twice; beta once. Delta exports too, but is not eligible. Gamma does not export.

| Stage | Expected result | Why |
|---|---|---|
| `eligible` | alpha, beta, gamma | Denominator is fixed by the eligibility snapshot |
| `adopters` | alpha, beta, delta | DISTINCT collapses repeated exports |
| Joined counts | 3 eligible, 2 adopting | Delta cannot enter the denominator; gamma stays |
| Final percentage | About 66.7% | Two of three eligible workspaces adopted |

To inspect a stage, keep the relevant `WITH` definitions and replace the final SELECT with, for example, `SELECT * FROM eligible;`. CTE names exist within that statement, not as tables available to the next independent query.

## Common mistakes

**Joining raw events directly.** Alpha's two exports could create two joined rows and distort both counts. Deduplicating at the intended grain avoids that specific problem.

**Filtering the joined activity in WHERE.** Requiring a matching event after the LEFT JOIN would remove non-adopters. The percentage might misleadingly become 100%.

**Assuming every eligible workspace had the same opportunity.** A workspace eligible for only one day may need a different measurement design. The example assumes the eligibility definition already handles opportunity and exclusions.

**Treating readability as a performance guarantee.** Ask Data or Engineering to inspect expensive production queries; optimizer behavior depends on the database and query.

## Questions to resolve first

Who belongs in the historical population? Is an export the intended value event? Are internal accounts excluded consistently? Can workspace IDs be missing or reused? Which stage can we reconcile against a trusted report?

Read [GROUP BY, WHERE and HAVING](group-by-where-and-having-for-product-managers.md) for aggregation and [activation](../Product-Metrics/activation-for-product-managers.md) for choosing a meaningful event.

## Technical reference

[PostgreSQL: WITH queries](https://www.postgresql.org/docs/current/queries-with.html) covers CTE scope, evaluation and materialization considerations.

---

### Make multi-stage analysis easier to review

**SQL for Product Managers** — follow Klyvero’s SQL examples to connect readable queries with trustworthy product decisions.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/sql-for-product-managers/)
