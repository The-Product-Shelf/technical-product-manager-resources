# GROUP BY, WHERE and HAVING for Product Managers

## Quick Reference

| Question | Clause | What it acts on |
|---|---|---|
| Which rows belong in the analysis? | `WHERE` | Rows before aggregation |
| What does one result row represent? | `GROUP BY` | The grouping dimensions |
| Which summaries should remain? | `HAVING` | Groups after aggregation |
| How many source rows are there? | `COUNT(*)` | Includes rows with NULL values |
| How many known values are there? | `COUNT(column)` | Excludes NULL in that column |
| How many different known users? | `COUNT(DISTINCT user_id)` | Distinct non-NULL IDs |

This is a guide to the logical meaning of the clauses, not the database engine's physical execution plan. SQL examples use syntax supported by PostgreSQL.

## Worked example: meaningful workspace activity

Klyvero wants to identify workspaces with at least two people creating projects during January. One event is not one person: repeat activity should not inflate the number of creators.

Assume `events(workspace_id, user_id, event_name, occurred_at)` contains:

| Workspace | User | Event | Time |
|---|---|---|---|
| alpha | u1 | project_created | 2026-01-05 10:00 |
| alpha | u1 | project_created | 2026-01-06 10:00 |
| alpha | u2 | project_created | 2026-01-07 10:00 |
| beta | u3 | project_created | 2026-01-08 10:00 |
| beta | u4 | project_viewed | 2026-01-09 10:00 |
| gamma | u5 | project_created | 2026-02-01 00:00 |

```sql
SELECT
    workspace_id,
    COUNT(*) AS creation_events,
    COUNT(DISTINCT user_id) AS creators
FROM events
WHERE event_name = 'project_created'
  AND occurred_at >= '2026-01-01 00:00:00'
  AND occurred_at <  '2026-02-01 00:00:00'
GROUP BY workspace_id
HAVING COUNT(DISTINCT user_id) >= 2
ORDER BY workspace_id;
```

The result is `alpha | 3 | 2`. The event and time filters exclude beta's view and gamma's February event. Beta then fails the group-level creator threshold. Alpha's two events from u1 count as two events but one creator.

The half-open time range includes the start and excludes the next period's start. Agree the reporting timezone and how `occurred_at` is stored before using these boundaries. The threshold of two is illustrative, not a definition of healthy adoption.

## Change the question deliberately

Adding `user_id` to `GROUP BY` would change the grain to workspace-and-user. It would no longer answer the same workspace question. Conversely, removing the event filter would count people who did anything, rather than people who created projects.

A group with no matching events does not appear in this result. If every workspace must appear, including zero-activity workspaces, begin with a workspace population and use an appropriate [LEFT JOIN](sql-joins-explained-for-product-managers.md). Do not accidentally eliminate its unmatched rows with a later event filter.

NULL user IDs also matter: `COUNT(*)` still counts their events, while `COUNT(DISTINCT user_id)` excludes those IDs. Decide whether anonymous events belong in the population; do not silently interpret them as authenticated creators.

## Review checklist

- State the grain in a sentence: “one row per workspace.”
- Check whether joins duplicate source events before counting them.
- Name the event and time window precisely.
- Compare a tiny hand-counted sample with the query.
- Explain why the `HAVING` threshold helps answer the product question.

Use [CTEs](ctes-for-product-managers.md) when several populations or stages make the query difficult to inspect. See the broader [SQL cheat sheet](sql-cheat-sheet-for-product-managers.md) for basic syntax.

## Technical reference

[PostgreSQL: table expressions](https://www.postgresql.org/docs/current/queries-table-expressions.html) explains grouping and the different roles of WHERE and HAVING.

## Related resources

- [Date and Time Queries for Product Managers](date-and-time-queries-for-product-managers.md) — Select a business day across daylight-saving boundaries without losing zero-activity days.

---

### Read summaries with confidence

**SQL for Product Managers** — practice turning Klyvero’s product questions into queries and checking what each result actually counts.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/sql-for-product-managers/)
