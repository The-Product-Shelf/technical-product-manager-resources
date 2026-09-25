# Finding Duplicates and Handling NULLs for PMs

## Diagnose before removing rows

Klyvero's task-completion count rises unexpectedly. Some events were delivered twice, some reuse an identifier with conflicting data, and some have no known user. Applying DISTINCT to the final chart could conceal several different problems.

First state the intended grain: **one row per logical task-completion event**, identified by an event ID under an agreed uniqueness scope. Repeated user IDs are normal when someone completes several tasks. Repeated event IDs need investigation, but even that rule depends on whether IDs are unique globally or only within a producer or workspace.

## Inspect a small synthetic dataset

The query runs unchanged in SQLite and uses PostgreSQL-compatible SQL. The fixture assumes non-NULL event IDs should be globally unique; real data may need a compound key.

```sql
WITH events(event_id, user_id) AS (
    VALUES
      ('e1', 'u1'), ('e1', 'u1'),
      ('e2', 'u2'), ('e2', 'u3'),
      ('e3', NULL),
      (NULL, 'u4'), (NULL, 'u4')
)
SELECT
    event_id,
    COUNT(*) AS rows_seen,
    COUNT(DISTINCT user_id) AS known_users,
    SUM(CASE WHEN user_id IS NULL THEN 1 ELSE 0 END) AS missing_user_rows
FROM events
GROUP BY event_id
ORDER BY CASE WHEN event_id IS NULL THEN 0 ELSE 1 END, event_id;
```

| Event ID | Rows seen | Known users | Missing-user rows |
|---|---:|---:|---:|
| NULL | 2 | 1 | 0 |
| e1 | 2 | 1 | 0 |
| e2 | 2 | 2 | 0 |
| e3 | 1 | 0 | 1 |

The NULL group is a bucket of records with unknown IDs, **not proof that they represent the same event**. `COUNT(DISTINCT user_id)` excludes NULL, so e3's zero means no known user in that group, not that the event had no actor.

## Decide which question each anomaly raises

| Pattern | Hypothesis | Evidence needed before correction |
|---|---|---|
| e1 repeats | Same event redelivered | Compare relevant payload, source and delivery metadata |
| e2 has two users | Reused ID, bad join or producer error | Establish the ID's scope and authoritative event |
| e3 lacks a user | Anonymous behavior or missing instrumentation | Check the event contract and identity lifecycle |
| Event ID missing | Producer omitted identity | Recover provenance; do not invent uniqueness from row similarity |

This query compares only IDs and users. Two rows can agree on both and still disagree on timestamp, task or workspace. Conversely, otherwise identical rows can represent legitimate repeated actions when no trustworthy event key exists.

## Why quick fixes can mislead

DISTINCT removes duplicates across the columns selected, not according to your business intent. Selecting only user and day can collapse two valid completions. Replacing NULL with a shared value like “unknown” can make many unidentified users look like one person. Filtering them out can shrink the denominator and improve a rate without improving the product.

Keep raw evidence through the team's approved data process and define a reviewed correction policy. If choosing a surviving record is justified, document precedence and tie-breaking; “take any row” can make results unstable. Product's role is to clarify the measurement consequence, not delete production records manually.

[Data models](../Software-Architecture/databases-and-product-data-models-for-pms.md) explains identity scope; [SQL JOINs](sql-joins-explained-for-product-managers.md) explains row multiplication; [metric-change investigation](../Product-Metrics/evaluating-a-metric-change-for-product-managers.md) connects these checks to a suspicious chart.

## Primary reference

[PostgreSQL's aggregate documentation](https://www.postgresql.org/docs/current/functions-aggregate.html) explains counting and NULL handling. Use `IS NULL` for missing-value checks rather than equality to NULL.

---

### Understand the records behind the result

**SQL for Product Managers** — work through Klyvero’s analytical questions and learn to investigate misleading counts before acting on them.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/sql-for-product-managers/)
