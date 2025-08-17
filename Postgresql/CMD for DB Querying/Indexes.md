- instead of scanning every page to find a word, the database jumps directly to the location
	- index scan -> Faster for lookup, join, and filtering
- Indexes speed up:
	- WHERE conditions
	- JOIN conditions
	- ORDER BY and GROUP BY (sometimes)
	- DISTINCT queries
- When Indexes Hurt
	- They take extra space on disk.
	- Inserts/updates/deletes are slower (because the index must be updated too).
	- Too many indexes = worse write performance.
- JUST INDEX WHAT YOU QUERY OFTEN

- Common types of Indexes
1. B-Tree Index
	- Great for equality and range queries (=,<,>, BETWEEN)
	```postgresql
		CREATE INDEX idx_customer_name ON customers(name);
	```

2. Hash Index
	- Optimized for equality lookups only
	```postgresql
			CREATE INDEX idx_customer_email_hash ON customers USING hash(email);
	```

3. GIN (Generalized Inverted Index)
	- Best for searching inside arrays, JSON and full-text search
```postgresql
			CREATE INDEX idx_doc_content_gin ON documents USING gin(to_tsvector('english', content));
```x