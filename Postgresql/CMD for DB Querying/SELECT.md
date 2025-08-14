- Retrieve data from a table
- Divided into: 
	- a select list (columns to be returned)
	- table list (tables from which to retrieve data)
	- optional qualification (specifies any restriction)

- All columns:
```sql
SELECT * FROM weather;
```

- certain columns:
```sql
SELECT city, temp_lo, temp_hi, prcp, date FROM weather;
```

- can add math expression into select list:
```sql
SELECT city, (temp_hi+temp_lo)/2 AS temp_avg, date FROM weather;
```

- `WHERE + AND` Clause:
```sql
SELECT * FROM weather  
   WHERE city = 'Hayward' AND temp_lo > 0;
```

- `ORDER BY` Clause:
```sql
  
postgres=> SELECT * FROM weather ORDER BY city, temp_lo;  
```

- remove duplicates
```sql
SELECT DISTINCT city FROM weather;
```