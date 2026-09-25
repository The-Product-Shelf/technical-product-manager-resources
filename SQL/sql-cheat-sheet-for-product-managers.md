# SQL Cheat Sheet for Product Managers

Turn a product question into a query, then check that the result answers the question you meant to ask.

## Quick Reference

| Need to… | Reach for… |
| --- | --- |
| Choose columns and rows | `SELECT ... FROM ... WHERE ...` |
| Sort and limit the result | `ORDER BY ... LIMIT ...` |
| Count actions | `COUNT(*)` on an events table |
| Count people | `COUNT(DISTINCT user_id)` |
| Compare groups | `GROUP BY`; filter the totals with `HAVING` |
| Find missing values | `IS NULL` |

**Before trusting a number:** check what one row represents, the population and the time window. `COUNT(DISTINCT user_id)` excludes NULL identifiers; a count alone is not an adoption rate.

## Using this cheat sheet

Klyvero is a fictional project management product. We want to investigate Project Templates: who uses the feature, how often, and whether the data is complete. The examples use PostgreSQL-compatible SQL and a small illustrative schema, not a production dataset. Date functions and other syntax can differ in your warehouse.

## Know what a row represents

| Table | One row represents | Fields used here |
| --- | --- | --- |
| `users` | One user | `user_id` (unique), `plan` (current plan) |
| `events` | One recorded action | `event_id` (unique), `user_id`, `event_name`, `occurred_at` (UTC timestamp) |

A person can generate many events. Counting events is not the same as counting users. These examples assume events have already been deduplicated and internal/test activity removed. Confirm the approved analysis environment and data definitions with Data before adapting them.

## The pieces you will use most

| SQL | Use it to | Watch for |
| --- | --- | --- |
| `SELECT ... FROM ...` | Choose columns and their table. | Select the information you need. |
| `WHERE` | Keep rows matching a condition. | Your filters define the population. |
| `AND`, `OR`, `IN` | Combine conditions or match a set. | Use parentheses to make mixed conditions explicit. |
| `ORDER BY` | Sort results. | Add a tie-breaker when order matters. |
| `LIMIT` | Restrict the number of returned rows. | It does not guarantee a cheap query or a representative sample. |
| `DISTINCT` | Remove duplicate combinations of selected values. | Distinct users and distinct events answer different questions. |
| `COUNT(*)` | Count rows. | A JOIN may multiply them. |
| `COUNT(column)` | Count non-NULL values. | Missing values are excluded. |
| `COUNT(DISTINCT column)` | Count distinct non-NULL values. | Unidentified users will be missing. |
| `SUM`, `AVG`, `MIN`, `MAX` | Summarize values. | Check units, missing values and the population. |
| `GROUP BY` | Summarize separately for each group. | The output now has one row per group. |
| `HAVING` | Filter groups after aggregation. | Use `WHERE` for input rows. |
| `IS NULL` | Find missing/unknown values. | `= NULL` does not test for missing data. |

Syntax reference: PostgreSQL's [SELECT](https://www.postgresql.org/docs/current/sql-select.html), [aggregate functions](https://www.postgresql.org/docs/current/functions-aggregate.html) and [comparison predicates](https://www.postgresql.org/docs/current/functions-comparison.html).

## Inspect recent recorded uses

```sql
SELECT event_id, user_id, occurred_at
FROM events
WHERE event_name = 'template_used'
ORDER BY occurred_at DESC, event_id DESC
LIMIT 20;
```

This returns the latest 20 matching events available in the table. It does not tell us how widely adopted the feature is. A single person could account for every row.

## Count uses and people separately

```sql
SELECT
    COUNT(*) AS template_uses,
    COUNT(DISTINCT user_id) AS users_using_templates
FROM events
WHERE event_name = 'template_used'
  AND occurred_at >= '2026-08-01 00:00:00'
  AND occurred_at <  '2026-09-01 00:00:00';
```

The start is included and the end excluded, covering August in the stated UTC convention without guessing the precision of the final second. If your business reports by local time, agree on how its calendar boundaries translate to stored timestamps.

If the result were 120 uses from 30 users, we would know frequency and reach within the recorded activity. We still would not know an adoption rate: that needs a defined eligible population as the denominator.

## Compare days with at least two recorded users

```sql
SELECT
    CAST(occurred_at AS DATE) AS activity_day,
    COUNT(DISTINCT user_id) AS users_using_templates
FROM events
WHERE event_name = 'template_used'
  AND occurred_at >= '2026-08-01 00:00:00'
  AND occurred_at <  '2026-09-01 00:00:00'
GROUP BY CAST(occurred_at AS DATE)
HAVING COUNT(DISTINCT user_id) >= 2
ORDER BY activity_day;
```

Here, `WHERE` selects August activity and `HAVING` removes days with fewer than two recorded users. Days without events will not appear; a missing row is not a displayed zero. Adding daily unique counts also does not produce monthly unique users, because someone may appear on multiple days.

## Check missing identifiers

```sql
SELECT COUNT(*) AS events_missing_user_id
FROM events
WHERE event_name = 'template_used'
  AND occurred_at >= '2026-08-01 00:00:00'
  AND occurred_at <  '2026-09-01 00:00:00'
  AND user_id IS NULL;
```

If this count rises, a falling unique-user count may reflect tracking changes. Investigate before concluding that adoption has dropped.

## Questions worth asking Data

- What does one row represent, and how are duplicates handled?
- Which timezone and event timestamp should this question use?
- Are internal users, deleted accounts or late events included?
- Which denominator turns this count into a meaningful rate?
- Can we compare the result with a trusted dashboard or known example?

To combine user attributes with activity, use [SQL JOINs Explained](sql-joins-explained-for-product-managers.md). To define rates, see the [product metrics cheat sheet](../Product-Metrics/product-metrics-cheat-sheet.md).

---

### Build confidence with product queries

This resource is part of The Product Shelf's free Technical Product Management library.

**SQL for Product Managers**

Practice turning Klyvero’s product questions into queries, then use the results to investigate adoption, funnels and changes in behavior.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/sql-for-product-managers/)
