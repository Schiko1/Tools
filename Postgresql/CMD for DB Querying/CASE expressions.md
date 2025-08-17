- SELECT returns a single value chosen from branches

1. Searched CASE
```postgresql
SELECT
	CASE
		WHEN amount >= 1000 THEN 'enterprise'
		WHEN amount >= 100 THEN 'mid'
		ELSE 'smaller'
	END AS tier
FROM orders;
```

2. Simple CASE
```postgresql
SELECT
  CASE region
    WHEN 'EMEA' THEN 1
    WHEN 'APAC' THEN 2
    ELSE 3
  END AS region_code
FROM customers;
```

```postgresql
-- Revenue by segment in one pass
SELECT
  SUM(CASE WHEN amount >= 1000 THEN amount ELSE 0 END) AS enterprise_rev,
  SUM(CASE WHEN amount >= 100  THEN amount ELSE 0 END) AS mid_rev
FROM orders;


SELECT SUM(revenue) / NULLIF(SUM(visits), 0)::NUMERIC AS rev_per_visit
FROM daily_metrics;
```
