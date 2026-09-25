# Date and Time Queries for Product Managers

## Quick Reference

| Decision | Safe question to ask | Common mistake |
|---|---|---|
| Event versus ingestion time | When did the action happen, and when did we receive it? | Assigning a delayed event to the day it arrived |
| Reporting timezone | Which named timezone defines the business day? | Treating a fixed UTC offset as valid all year |
| Window boundaries | Is the start included and the end excluded? | Counting midnight in two adjacent windows |
| Timestamp representation | Are values normalized instants or local wall times? | Comparing mixed offsets as plain strings |
| Observation cutoff | Is the full period available, including agreed ingestion delay? | Comparing today's partial data with yesterday's full day |

Klyvero wants daily task activity for a Madrid-based workspace. A local calendar day can differ from 24 elapsed hours around a daylight-saving transition. Do not derive every next midnight by adding 24 hours in UTC.

## A runnable boundary example

This synthetic fixture uses fixed-width UTC text in `YYYY-MM-DD HH:MM:SS` format. Within this deliberately normalized format, lexical order matches chronological order. The query runs unchanged in SQLite, and its SQL syntax is compatible with PostgreSQL. The TEXT timestamp representation is deliberately chosen only for this self-contained, cross-engine teaching fixture; it is not a recommended PostgreSQL production pattern. Real PostgreSQL analysis should use appropriate `timestamp` / `timestamp with time zone` types and explicit timezone handling. It does **not** implement timezone conversion: the calendar boundaries are supplied explicitly, as they could be by a reviewed calendar table.

For `Europe/Madrid`, March 29, 2026 spans 23 elapsed hours: local midnight starts at March 28 23:00 UTC and the next midnight is March 29 22:00 UTC.

```sql
WITH days(day_label, start_utc, end_utc) AS (
    VALUES
      ('2026-03-28', '2026-03-27 23:00:00', '2026-03-28 23:00:00'),
      ('2026-03-29', '2026-03-28 23:00:00', '2026-03-29 22:00:00'),
      ('2026-03-30', '2026-03-29 22:00:00', '2026-03-30 22:00:00')
), events(event_id, occurred_utc) AS (
    VALUES
      ('before', '2026-03-27 22:59:59'),
      ('e1',     '2026-03-27 23:00:00'),
      ('e2',     '2026-03-28 23:00:00'),
      ('e3',     '2026-03-29 21:59:59'),
      ('after',  '2026-03-30 22:00:00')
)
SELECT d.day_label, COUNT(e.event_id) AS events
FROM days d
LEFT JOIN events e
  ON e.occurred_utc >= d.start_utc
 AND e.occurred_utc <  d.end_utc
GROUP BY d.day_label
ORDER BY d.day_label;
```

| Day | Expected events |
|---|---:|
| 2026-03-28 | 1 |
| 2026-03-29 | 2 |
| 2026-03-30 | 0 |

The event exactly at March 28 23:00 belongs only to March 29's local day. The first and last fixture events fall outside the supplied windows. The LEFT JOIN preserves the zero-activity day, while counting the non-NULL event identifier avoids counting its unmatched placeholder row.

## Apply the idea to real timestamps

Production data may use native timestamp types, offsets, fractional seconds or warehouse-specific functions. Replace the teaching representation with the team's supported types and timezone conversion; do not copy text comparisons onto mixed-format data. PostgreSQL's `timestamp with time zone` represents an instant but does not preserve the original named timezone as part of that value. Keep the business timezone separately when the product needs it.

Confirm treatment of ambiguous local times when clocks move backward and nonexistent times when they move forward. For completed-day reports, also decide when late events cause historical totals to be revised.

Use [retention queries](retention-and-cohort-queries-for-product-managers.md) for observation maturity and [GROUP BY](group-by-where-and-having-for-product-managers.md) for aggregate populations.

## Primary reference

[PostgreSQL's date/time types](https://www.postgresql.org/docs/current/datatype-datetime.html) documents timestamp and timezone behavior. SQLite uses different date/time facilities; this fixture avoids claiming those functions are interchangeable.

---

### Make time windows part of the question

**SQL for Product Managers** — practice Klyvero’s SQL analysis with explicit populations, dates and assumptions instead of trusting a plausible total.

→ [Continue learning at The Product Shelf](https://theproductshelf.com/product/sql-for-product-managers/)
