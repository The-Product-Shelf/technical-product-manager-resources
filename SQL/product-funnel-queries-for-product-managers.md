# Product Funnel Queries for Product Managers

## Define the funnel before writing SQL

For Klyvero, this illustrative funnel is **start setup → create a project → invite a teammate**. Count users, use each user's first observed setup start as the anchor, require strictly later steps, and allow seven days from that start. This is one funnel definition, not a universal convention.

The self-contained fixture below expresses time as integer hours from a shared observation origin. That keeps the example runnable in PostgreSQL and SQLite without timezone or interval syntax differences. Production queries should use real event timestamps and explicitly agreed timezone, late-arrival and identity rules.

Only users with a complete seven-day opportunity are included. At observation hour 240, their start must be at or before hour 72. The conversion window is `[start, start + 168)`: an event exactly at the upper boundary is excluded.

## Runnable example

```sql
WITH events(user_id, event_name, event_hour) AS (
    VALUES
      ('u1', 'start',   0), ('u1', 'create',   1),
      ('u1', 'create',  1), ('u1', 'invite',   2),
      ('u2', 'start',   0), ('u2', 'invite',   1),
      ('u2', 'create',  2),
      ('u3', 'start',   0), ('u3', 'create',   1),
      ('u3', 'invite',168),
      ('u4', 'start',   0), ('u4', 'create',   0),
      ('u5', 'start', 100), ('u5', 'create', 101),
      ('u5', 'invite',102)
), starts AS (
    SELECT user_id, MIN(event_hour) AS start_hour
    FROM events
    WHERE event_name = 'start'
    GROUP BY user_id
    HAVING MIN(event_hour) <= 72
), creations AS (
    SELECT s.user_id, s.start_hour, MIN(e.event_hour) AS create_hour
    FROM starts s
    LEFT JOIN events e
      ON e.user_id = s.user_id
     AND e.event_name = 'create'
     AND e.event_hour > s.start_hour
     AND e.event_hour < s.start_hour + 168
    GROUP BY s.user_id, s.start_hour
), invitations AS (
    SELECT c.user_id, c.create_hour, MIN(e.event_hour) AS invite_hour
    FROM creations c
    LEFT JOIN events e
      ON e.user_id = c.user_id
     AND e.event_name = 'invite'
     AND e.event_hour > c.create_hour
     AND e.event_hour < c.start_hour + 168
    GROUP BY c.user_id, c.create_hour
)
SELECT
    COUNT(*) AS started,
    COUNT(create_hour) AS created,
    COUNT(invite_hour) AS invited,
    100.0 * COUNT(invite_hour) / NULLIF(COUNT(*), 0) AS completion_pct
FROM invitations;
```

Expected result: **4 started, 3 created, 1 invited, 25% completion**.

| User | Interpretation |
|---|---|
| u1 | Completes; duplicate creation does not add another user |
| u2 | Does not complete; the invitation happened before creation |
| u3 | Does not complete; invitation is exactly outside the window |
| u4 | Does not reach creation; a timestamp tie fails strict ordering |
| u5 | Excluded entirely; the full observation window has not elapsed |

## What the query deliberately does not claim

The first observed start is not necessarily the user's first-ever start if source history is truncated. A repeated start does not reset this funnel's clock. Anonymous-to-signed-in identity stitching is not implemented. Hour-level timestamps are too coarse for many real workflows; tied timestamps need an explicit sequencing policy and reliable event precision.

The query selects the earliest qualifying creation, then checks for a later invitation. For these linear conditions, this preserves an opportunity for a qualifying later invitation. More complicated funnels with repeated sessions, exclusions or mutually dependent steps may need another design.

The overall rate is `1 / 4`; the creation-to-invitation step rate is `1 / 3`. Label the denominator when sharing either number. A missing event can also reflect instrumentation failure rather than user abandonment.

## Before using the result

Confirm the event contracts with Data, inspect example user journeys, and test duplicates, boundary times, empty cohorts and out-of-order steps. A zero-size cohort returns NULL for the percentage; it should not be displayed as a measured 0% conversion.

[CTEs](ctes-for-product-managers.md) explain the staged query structure. [Activation](../Product-Metrics/activation-for-product-managers.md) helps evaluate whether completing the funnel represents user value.

## Technical reference

[PostgreSQL: WITH queries](https://www.postgresql.org/docs/current/queries-with.html) documents the named query stages used here.

---

### Turn product journeys into inspectable queries

**SQL for Product Managers** — use Klyvero’s SQL examples to connect event data, joins and analysis assumptions with product questions.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/sql-for-product-managers/)
