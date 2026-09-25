# SQL JOINs Explained for Product Managers

How do we combine user information with activity without accidentally changing what we count?

Klyvero is a fictional project management product. Its `users` table describes people; its `events` table records actions. To investigate Project Templates by plan, we need information from both.

These are illustrative tables using the same schema as the [SQL cheat sheet](sql-cheat-sheet-for-product-managers.md). All example events occur in August 2026, and the queries use PostgreSQL-compatible syntax.

## Two tables, two kinds of row

**users**

| user_id | plan |
| --- | --- |
| 101 | Pro |
| 102 | Free |
| 103 | Pro |

**events**

| event_id | user_id | event_name |
| --- | --- | --- |
| 1 | 101 | template_used |
| 2 | 101 | template_used |
| 3 | 103 | project_created |
| 4 | 999 | template_used |

User 101 used Templates twice. User 102 has no recorded events. Event 4 refers to a user absent from this user table; we must investigate the mismatch rather than silently assume it is valid or invalid activity.

## INNER JOIN: keep matching pairs

```sql
SELECT u.user_id, u.plan, e.event_id
FROM users AS u
INNER JOIN events AS e ON e.user_id = u.user_id
WHERE e.event_name = 'template_used'
  AND e.occurred_at >= '2026-08-01 00:00:00'
  AND e.occurred_at <  '2026-09-01 00:00:00'
ORDER BY e.event_id;
```

| user_id | plan | event_id |
| --- | --- | --- |
| 101 | Pro | 1 |
| 101 | Pro | 2 |

The result contains two matching events from one user. Users without matching template events are absent, and event 4 cannot match a user.

An inner join is useful when we need the overlap. It can mislead us if we expected the entire user population to remain available for a denominator.

## LEFT JOIN: keep the left-hand population

```sql
SELECT u.user_id, u.plan, COUNT(e.event_id) AS template_uses
FROM users AS u
LEFT JOIN events AS e
  ON e.user_id = u.user_id
  AND e.event_name = 'template_used'
  AND e.occurred_at >= '2026-08-01 00:00:00'
  AND e.occurred_at <  '2026-09-01 00:00:00'
GROUP BY u.user_id, u.plan
ORDER BY u.user_id;
```

| user_id | plan | template_uses |
| --- | --- | --- |
| 101 | Pro | 2 |
| 102 | Free | 0 |
| 103 | Pro | 0 |

We kept all three users. For users without a matching event, the event columns are NULL. `COUNT(e.event_id)` counts matched events; `COUNT(*)` would also count the preserved unmatched row and return 1 for each of those users.

The event filters are inside `ON` deliberately. Moving `e.event_name = 'template_used'` into `WHERE` would discard the unmatched rows because their event name is NULL. PostgreSQL's [join tutorial](https://www.postgresql.org/docs/current/tutorial-join.html) explains the matching behavior; its [table expressions reference](https://www.postgresql.org/docs/current/queries-table-expressions.html) covers filtering and outer joins.

## The multiplication trap

A join creates a row for each matching pair. If one user has two events and three projects, joining both detail tables directly through the user can produce six rows. Summing event or project amounts over those rows can inflate the result.

`COUNT(DISTINCT user_id)` can answer a unique-user question, but it does not repair every duplicated sum. Often the right approach is to summarize each detail table to one row per user before joining. Ask Data to help when multiple one-to-many relationships are involved.

## What does “by plan” mean?

In this example, `users.plan` is the current plan. Combining it with August events groups historical behavior by today's plan. It does not reveal the plan at the time of the event.

That may be exactly what we want when targeting current customers. For a historical comparison between Free and Pro behavior, we need the plan at the event time or an appropriate history table. A query can run successfully while answering the wrong version of the question.

## Questions worth asking Data

- Is each join key unique on the side where we expect it to be?
- Which users or events disappear without a match?
- Are we preserving the intended denominator?
- Does each attribute describe the present or the time of the event?
- How many rows and distinct users exist before and after the join?

---

### Use joined data to answer product questions

This resource is part of The Product Shelf's free Technical Product Management library.

**SQL for Product Managers**

Work through Klyvero examples that connect users and activity, check what the results count, and apply SQL to product analysis.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/sql-for-product-managers/)
