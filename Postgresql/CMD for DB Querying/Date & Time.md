- Types
	- DATE: calendar date without time
	- TIME {WITHOUT TIME ZONE} - time of day without date
	- TIME WITH TIME ZONE
	- TIMESTAMP WITHOUT TIME ZONE - date + Time
	- TIMESTAMPTZ - stored in UTC, displayed in your session time zone -> for events
	- INTERVAL - e.g. 3 days, 2 hours

```sql
SELECT now(), statement_timestamp(), clock_timestamp(), current_date;
```

### 1.3 Converting time zones

Two common ways:

1. Function form:
    

`-- Show Cairo local time for a timestamptz SELECT timezone('Africa/Cairo', placed_at) AS placed_local FROM orders;  -- Build a timestamptz from a "naive" timestamp by asserting its zone SELECT timestamptz '2025-08-17 09:00' AT TIME ZONE 'UTC'; -- returns timestamp without tz in UTC wall-time`

2. `AT TIME ZONE` operator (direction depends on input type):
    

- `timestamp WITHOUT tz AT TIME ZONE 'Zone'` → `timestamptz` (assume the timestamp is in that zone; convert to UTC).
    
- `timestamptz AT TIME ZONE 'Zone'` → `timestamp WITHOUT tz` (render as local wall time in that zone).
    

Examples:

`-- Naive sales timestamp recorded as local New York time; store as UTC: SELECT (ts_without_tz AT TIME ZONE 'America/New_York')::timestamptz;  -- Display UTC timestamptz in Cairo local wall time (drop tz): SELECT (placed_at AT TIME ZONE 'Africa/Cairo') AS cairo_wall_time;`

### 1.4 Extracting parts

`SELECT   EXTRACT(YEAR  FROM placed_at) AS yr,   EXTRACT(MONTH FROM placed_at) AS mon,   EXTRACT(DAY   FROM placed_at) AS day,   EXTRACT(DOW   FROM placed_at) AS dow,     -- 0=Sunday..6=Saturday   EXTRACT(ISODOW FROM placed_at) AS isodow, -- 1=Mon..7=Sun   EXTRACT(WEEK  FROM placed_at) AS wk,      -- beware vs ISO week   EXTRACT(EPOCH FROM placed_at) AS epoch_seconds;`

`date_part('field', ts)` is an alias for `EXTRACT`.

### 1.5 Truncating/rounding time

`-- Bucket events into hours/days/months SELECT date_trunc('hour', placed_at) AS hour_bucket, COUNT(*) FROM orders GROUP BY 1 ORDER BY 1;`

Common grains: `'minute'|'hour'|'day'|'week'|'month'|'quarter'|'year'`.  
`date_trunc('week', ...)` starts at Monday 00:00.

### 1.6 Arithmetic & intervals

- `timestamp - timestamp` → `interval`
    
- `timestamp + interval` → `timestamp`
    
- `date + integer` → `date` (days)
    
- `age(ts1, ts2)` → “calendar” interval (months/days adjusted)
    

`-- Lead time in hours SELECT EXTRACT(EPOCH FROM (shipped_at - placed_at)) / 3600 AS lead_hours FROM orders;  -- End of month SELECT (date_trunc('month', placed_at) + INTERVAL '1 month')::date - 1 AS month_end FROM orders;  -- N days after current_date SELECT current_date + 7 AS next_week;`

Helpers:

`SELECT make_date(2025, 8, 17); SELECT make_time(14, 30, 0); SELECT make_timestamp(2025, 8, 17, 14, 30, 0); SELECT make_interval(days => 10, hours => 5);`

Normalize “weird” intervals:

`SELECT justify_days(INTERVAL '35 days');   -- 1 mon 5 days SELECT justify_hours(INTERVAL '27 hours'); -- 1 day 3 hours`

### 1.7 Formatting & parsing

`-- Format SELECT to_char(placed_at, 'YYYY-MM-DD HH24:MI:SS TZ');  -- Parse SELECT to_timestamp('2025-08-17 14:30:00', 'YYYY-MM-DD HH24:MI:SS'); SELECT to_date('17/08/2025', 'DD/MM/YYYY');`

Common format tokens: `YYYY, MM, DD, HH24, MI, SS, TZ, "Mon", "Dy", IW (ISO week)`.

Epoch conversions:

`SELECT to_timestamp(1_726_887_000);                 -- seconds → timestamptz SELECT EXTRACT(EPOCH FROM now());                   -- timestamptz → seconds`

### 1.8 Generating time series

`-- Hourly buckets for last 48 hours SELECT gs AS hour_bucket FROM generate_series(   date_trunc('hour', now()) - INTERVAL '47 hours',   date_trunc('hour', now()),   INTERVAL '1 hour' ) AS gs;  -- Left join facts to the series for dense time charts SELECT gs.hour_bucket, COALESCE(counts.cnt, 0) AS orders FROM (   SELECT generate_series(     date_trunc('day', now()) - INTERVAL '29 days',     date_trunc('day', now()),     INTERVAL '1 day'   ) AS hour_bucket ) gs LEFT JOIN (   SELECT date_trunc('day', placed_at) AS d, COUNT(*) AS cnt   FROM orders   GROUP BY 1 ) counts ON counts.d = gs.hour_bucket ORDER BY gs.hour_bucket;`

### 1.9 Grouping and index-friendly filters (important)

Avoid wrapping the column in functions in `WHERE` clauses (it can prevent index use). Prefer range predicates:

`-- Good (sargable) WHERE placed_at >= TIMESTAMPTZ '2025-08-01'   AND placed_at <  TIMESTAMPTZ '2025-09-01'  -- Risky (may skip index) WHERE date_trunc('month', placed_at) = TIMESTAMPTZ '2025-08-01 00:00:00+02'`

If you often group on `date_trunc('day', placed_at)`, consider a **generated column** or **functional index**:

`CREATE INDEX ON orders (date_trunc('day', placed_at));`