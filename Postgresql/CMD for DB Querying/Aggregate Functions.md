- max()/min()
```sql
SELECT max(temp_lo) FROM weather;
```

- To get the city that has the lowest temp, the max(temp_lo) need to be put in a subquery after the WHERE clause
	- Find max temp_lo and sees if weather has that value, then gives the city

```sql
SELECT city FROM weather WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
```

- COUNT()
	- 