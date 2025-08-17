1. `EXPLAIN` and `EXPLAIN ANALYZE`
	- shows how postgresql executes queries
```postgresql
EXPLAIN ANALYZE
SELECT * FROM orders WHERE customer_id = 123;
```
	- see whether it used seq scan or Index scan
	- see cost and actual runtime

2. Avoid SELECT *

3. Use Joins Efficiently
	- make sure join columns are indexed

4. LIMIT Results

5. Proper Data Types

6. Postgres needs statistics to optimize queries
	- ANALYZE updates table stats
	- VACUUM cleans up dead rows
```postgresql
	VACUUM ANALYZE;
```

7. Avoid correlated subqueries
	- don't
	```postgresql
SELECT c.name, (SELECT COUNT(*) FROM orders o WHERE o.customer_id = c.customer_id) FROM customers c;
	```
	- do
```postgresql
SELECT c.name, COUNT(o.order_id) AS order_count
FROM customers c
LEFT JOIN orders o ON o.customer_id = c.customer_id
GROUP BY c.name;
```

7. Use `EXISTS` instead of `IN` for large sets
	- don't
```postgresql
SELECT * FROM orders WHERE customer_id IN (SELECT id FROM customers);
```
	- do
```postgresql
SELECT * FROM orders o WHERE EXISTS (
  SELECT 1 FROM customers c WHERE c.id = o.customer_id
);
```