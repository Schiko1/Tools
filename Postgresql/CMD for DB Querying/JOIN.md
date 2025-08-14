

- Basic (inner) join:
	- combine rows from two tables, with specification of which columns need to match
```sql
SELECT * FROM weather JOIN cities ON city = name; -- column names > *
```

- If there is not matching entry in cities.name to weather.city -> unmatched row of cities.name will not be added to join

```sql
SELECT weather.city, weather.date, cities.location FROM weather JOIN cities ON weather.city = cities.name; 
```

- Left (outer) join:
	- combines rows from two tables, shows all rows from the left table and matching rows from right table
		- If no match is found, Null values for each column of table 2 are returned

```sql
SELECT *
    FROM weather LEFT OUTER JOIN cities ON weather.city = cities.name;
```

- Self-Join:
	- used for comparison of each row's column value
	- Also see renaming of tables to `w1` annd `w2`

```sql
SELECT w1.city, w1.temp_lo AS low, w1.temp_hi AS high,
       w2.city, w2.temp_lo AS low, w2.temp_hi AS high
    FROM weather w1 JOIN weather w2
        ON w1.temp_lo < w2.temp_lo AND w1.temp_hi > w2.temp_hi;
        --- What cities have a lower low and a higher high
```

- FULL (OUTER) JOIN
	- returns all rows when there is a match in either left or right table
	- both left join and right join combined