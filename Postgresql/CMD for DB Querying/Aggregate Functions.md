- max()/min()
```sql
SELECT max(temp_lo) FROM weather;
```

- To get the city that has the lowest temp, the max(temp_lo) need to be put in a subquery after the WHERE clause
	- Find max temp_lo and sees if weather has that value, then gives the city

```sql
SELECT city FROM weather WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
```

- COUNT() + GROUP BY
```sql
SELECT city, count(*), max(temp_lo) FROM weather GROUP BY city;
```

- additionally, filter rows with HAVING
```sql
SELECT city, count(*), max(temp_lo)
    FROM weather
    GROUP BY city
    HAVING max(temp_lo) < 40;
```
- Difference between WHERE and HAVING
	- WHERE selects inputs rows before groups and aggregates are computed
		- selects which row goes into the aggregate function
		- WHERE itself must not contain aggregate functions, IT IS NOT OPTIMAL
	- HAVING selects group rows after groups and aggregates are computed
		- Always contain aggregate functions(like max, min,etc.)

- LIKE Operator for string comparison
```sql
SELECT city, count(*), max(temp_lo)
    FROM weather
    WHERE city LIKE 'S%'            -- (1)
    GROUP BY city;
```

- FILTER
	- Another way to select rows that go into an aggregate computation
	- per-aggregate option
		- removes rows only from the input of the particular aggregate function that it is attached to
		- Here, it only counts the rows with temp_lo that is less than 45
```sql
SELECT city, count(*) FILTER (WHERE temp_lo < 45), max(temp_lo)
    FROM weather
    GROUP BY city;
```

- Other functions:
	- avg(expression)
	- sum(expression)
	- bit_and(expression)
		- the bitwise AND of all non-null input values, or null if none
	- bit_or(expression)
	- bool_and(expression)
		- true if all input values are true, otherwise false
	- bool_or(expression)


## STATISTICS FUNCTIONS

|Function|Argument Type|Return Type|Description|
|---|---|---|---|
|`corr(Y, X)`|double precision|double precision|correlation coefficient|
|`covar_pop(Y, X)`|double precision|double precision|population covariance|
|`covar_samp(Y, X)`|double precision|double precision|sample covariance|
|`regr_avgx(Y, X)`|double precision|double precision|average of the independent variable (sum(X)/N)|
|`regr_avgy(Y, X)`|double precision|double precision|average of the dependent variable (sum(Y)/N)|
|`regr_count(Y, X)`|double precision|bigint|number of input rows in which both expressions are nonnull|
|`regr_intercept(Y, X)`|double precision|double precision|y-intercept of the least-squares-fit linear equation determined by the (X, Y) pairs|
|`regr_r2(Y, X)`|double precision|double precision|square of the correlation coefficient|
|`regr_slope(Y, X)`|double precision|double precision|slope of the least-squares-fit linear equation determined by the (X, Y) pairs|
|`regr_sxx(Y, X)`|double precision|double precision|sum(X^2) - sum(X)^2/N ("sum of squares" of the independent variable)|
|`regr_sxy(Y, X)`|double precision|double precision|sum(X*Y) - sum(X) * sum(Y)/N ("sum of products" of independent times dependent variable)|
|`regr_syy(Y, X)`|double precision|double precision|sum(Y^2) - sum(Y)^2/N ("sum of squares" of the dependent variable)|
|`stddev(expression)`|smallint, int, bigint, real, double precision, or numeric|double precision for floating-point arguments, otherwise numeric|historical alias for `stddev_samp`|
|`stddev_pop(expression)`|smallint, int, bigint, real, double precision, or numeric|double precision for floating-point arguments, otherwise numeric|population standard deviation of the input values|
|`stddev_samp(expression)`|smallint, int, bigint, real, double precision, or numeric|double precision for floating-point arguments, otherwise numeric|sample standard deviation of the input values|
|`variance`(expression)|smallint, int, bigint, real, double precision, or numeric|double precision for floating-point arguments, otherwise numeric|historical alias for `var_samp`|
|`var_pop`(expression)|smallint, int, bigint, real, double precision, or numeric|double precision for floating-point arguments, otherwise numeric|population variance of the input values (square of the population standard deviation)|
|`var_samp`(expression)|smallint, int, bigint, real, double precision, or numeric|double precision for floating-point arguments, otherwise numeric|sample variance of the input values (square of the sample standard deviation)|

### Ordered-SET Aggregated Function

| Function                                                             | Direct Argument Type(s) | Aggregated Argument Type(s)  | Return Type                     | Description                                                                                                                                                                                      | Example Query                                                                                                             |
| -------------------------------------------------------------------- | ----------------------- | ---------------------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| `mode() WITHIN GROUP (ORDER BY sort_expression)`                     | *(none)*                | any sortable type            | same as sort expression         | returns the most frequent input value (arbitrarily choosing the first one if there are multiple equally-frequent results)                                                                        | ```sql\nSELECT mode() WITHIN GROUP (ORDER BY score) AS most_common_score\nFROM scores;\n```                               |
| `percentile_cont(fraction) WITHIN GROUP (ORDER BY sort_expression)`  | double precision        | double precision or interval | same as sort expression         | continuous percentile: returns a value corresponding to the specified fraction in the ordering, interpolating between adjacent input items if needed                                             | ```sql\nSELECT percentile_cont(0.5) WITHIN GROUP (ORDER BY score) AS median_score\nFROM scores;\n```                      |
| `percentile_cont(fractions) WITHIN GROUP (ORDER BY sort_expression)` | double precision[]      | double precision or interval | array of sort expression's type | multiple continuous percentile: returns an array of results matching the shape of the fractions parameter, with each non-null element replaced by the value corresponding to that percentile     | ```sql\nSELECT percentile_cont(ARRAY[0.25, 0.5, 0.75]) WITHIN GROUP (ORDER BY score) AS quartiles\nFROM scores;\n```      |
| `percentile_disc(fraction) WITHIN GROUP (ORDER BY sort_expression)`  | double precision        | any sortable type            | same as sort expression         | discrete percentile: returns the first input value whose position in the ordering equals or exceeds the specified fraction                                                                       | ```sql\nSELECT percentile_disc(0.5) WITHIN GROUP (ORDER BY score) AS median_score_disc\nFROM scores;\n```                 |
| `percentile_disc(fractions) WITHIN GROUP (ORDER BY sort_expression)` | double precision[]      | any sortable type            | array of sort expression's type | multiple discrete percentile: returns an array of results matching the shape of the fractions parameter, with each non-null element replaced by the input value corresponding to that percentile | ```sql\nSELECT percentile_disc(ARRAY[0.25, 0.5, 0.75]) WITHIN GROUP (ORDER BY score) AS quartiles_disc\nFROM scores;\n``` |
