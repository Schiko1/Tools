```sql
CREATE TABLE weather (
	city      varchar(80),
	temp_lo   int,              --low temperature
	temp_hi   int,
	prcp      real,
	date      date
);
```

```sql
DROP TABLE cities;
```

- Filling rows
```sql
INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
INSERT INTO cities VALUES ('San Francisco', '(-194.0, 53.0)');
SELECT * FROM weather;
```
- If some columns are unknown(here precipitation)
```sql
INSERT INTO weather (date, city, temp_hi, temp_lo)
    VALUES ('1994-11-29', 'Hayward', 54, 37);
```
- Explicitly listing the columns is better style

- load large amounts of data from flat-text files
```sql
COPY weather FROM '/home/user/weather.txt';
```
- Usually faster than INSERT