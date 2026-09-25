# Retention and Cohort Queries for Product Managers

## Turn a retention definition into a population

Klyvero wants **Week 1 task retention** among users assigned to a signup-week cohort. Week 1 is the calendar week after signup week, in UTC here; it is not the next seven days after each person's signup. Count each returning user once and include only cohorts whose entire return window has elapsed.

The synthetic `members` table is an already-reviewed cohort assignment, with one row per user and fixed eligibility. The `windows` table supplies half-open return intervals. This isolates population and maturity logic from database-specific calendar functions; see [date and time queries](date-and-time-queries-for-product-managers.md) for boundary construction.

## Runnable example

Timestamps use consistent fixed-width UTC text so the same query executes in SQLite and uses PostgreSQL-compatible SQL. Real schemas should use the warehouse's appropriate timestamp types. The observation cutoff is exclusive and assumes all earlier events are available; production reports also need an ingestion-completeness policy.

```sql
WITH windows(cohort, return_start, return_end) AS (
    VALUES
      ('2026-01-05', '2026-01-12 00:00:00', '2026-01-19 00:00:00'),
      ('2026-01-12', '2026-01-19 00:00:00', '2026-01-26 00:00:00')
), members(user_id, cohort) AS (
    VALUES ('u1', '2026-01-05'), ('u2', '2026-01-05'),
           ('u3', '2026-01-05'), ('u4', '2026-01-12')
), events(user_id, event_name, occurred_utc) AS (
    VALUES
      ('u1', 'task_completed', '2026-01-12 00:00:00'),
      ('u1', 'task_completed', '2026-01-13 10:00:00'),
      ('u2', 'task_completed', '2026-01-18 23:59:59'),
      ('u3', 'task_completed', '2026-01-19 00:00:00'),
      ('u4', 'task_completed', '2026-01-19 12:00:00')
), mature_members AS (
    SELECT m.user_id, m.cohort, w.return_start, w.return_end
    FROM members m
    JOIN windows w ON w.cohort = m.cohort
    WHERE w.return_end <= '2026-01-20 00:00:00'
), per_user AS (
    SELECT m.cohort, m.user_id,
           MAX(CASE WHEN e.user_id IS NOT NULL THEN 1 ELSE 0 END) AS returned
    FROM mature_members m
    LEFT JOIN events e
      ON e.user_id = m.user_id
     AND e.event_name = 'task_completed'
     AND e.occurred_utc >= m.return_start
     AND e.occurred_utc < m.return_end
    GROUP BY m.cohort, m.user_id
)
SELECT cohort, COUNT(*) AS cohort_users, SUM(returned) AS returned_users,
       100.0 * SUM(returned) / NULLIF(COUNT(*), 0) AS retention_pct
FROM per_user
GROUP BY cohort
ORDER BY cohort;
```

Expected output: **2026-01-05 | 3 cohort users | 2 returned users | 66.666…%**, or **66.7%** when rounded for display. The later cohort is omitted because it is immature; it is not measured at 0%.

## Reconcile the users

u1 returns twice but counts once. u2 returns just before the upper boundary and counts. u3's event is exactly at the next interval's start, so does not count in Week 1. u4 has activity but not a complete observation window. The LEFT JOIN retains u3 in the denominator despite no qualifying return.

If no cohort is mature, the query produces no rows. If a mature cohort has members but no returns, it produces zero returned users and 0%. Those are different reporting states. A chart can show immature cohorts as “not yet observed,” but should not fill their rate with zero.

## Before using this on product data

Check member uniqueness, stable identity and eligibility independently of return activity. Do not define the cohort by joining only to return events; that would exclude non-returners. Account merges and deleted identities need an agreed historical policy, and duplicate delivery must not create extra people.

This computes exact-interval retention, not return-on-or-after or consecutive-week retention. Read [retention and cohorts](../Product-Metrics/retention-and-cohorts-for-product-managers.md) before changing the definition. [CTEs](ctes-for-product-managers.md) explains the stages, and [duplicate diagnosis](finding-duplicates-and-handling-nulls-for-pms.md) helps check the input.

## Primary reference

[PostgreSQL's WITH documentation](https://www.postgresql.org/docs/current/queries-with.html) describes the staged query structure used here.

---

### Connect cohort definitions with reproducible analysis

**SQL for Product Managers** — use Klyvero’s SQL scenarios to inspect populations, joins and the meaning of a product metric.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/sql-for-product-managers/)
