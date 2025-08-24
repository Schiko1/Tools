## 1. What is a Subquery?
A **subquery** is a SQL query embedded inside another SQL query.  
It is enclosed in parentheses `(...)` and can appear in several places:
- `WHERE` clause (filtering rows)
- `FROM` clause (treating a subquery as a derived table)
- `SELECT` clause (returning computed scalar values)

---

## 2. Types of Subqueries
Subqueries can be classified based on:

### A) **Return type**
1. **Scalar Subquery**  
   - Returns exactly **one value (one row, one column)**.  
   - Often used in the `SELECT` or `WHERE` clause.
   ```sql
   SELECT first_name, last_name,
          (SELECT AVG(salary) FROM employees) AS avg_salary
   FROM employees;
   ```

2. **Row Subquery**  
   - Returns a single **row with multiple columns**.  
   - Typically used with row constructors or comparison operators.
   ```sql
   SELECT *
   FROM employees
   WHERE (department_id, salary) =
         (SELECT department_id, MAX(salary)
          FROM employees
          GROUP BY department_id
          LIMIT 1);
   ```

3. **Table Subquery (Derived Table / Inline View)**  
   - Returns multiple rows and columns.  
   - Treated like a virtual table in the `FROM` clause.
   ```sql
   SELECT dept_name, avg_salary
   FROM (
       SELECT department_id, AVG(salary) AS avg_salary
       FROM employees
       GROUP BY department_id
   ) AS dept_avg
   JOIN departments d ON d.department_id = dept_avg.department_id;
   ```

---

### B) **Execution dependency**
1. **Uncorrelated Subquery**  
   - Can run independently of the outer query.  
   - Executes **once**, and the result is reused by the outer query.
   ```sql
   SELECT first_name, last_name
   FROM employees
   WHERE salary > (SELECT AVG(salary) FROM employees);
   ```

2. **Correlated Subquery**  
   - Depends on values from the outer query.  
   - Executes **once per row** of the outer query.  
   ```sql
   SELECT e1.first_name, e1.salary
   FROM employees e1
   WHERE e1.salary > (
       SELECT AVG(e2.salary)
       FROM employees e2
       WHERE e2.department_id = e1.department_id
   );
   ```

---

## 3. Subqueries in Different Clauses

### A) **In WHERE / HAVING**
```sql
-- Employees earning more than the company average
SELECT first_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Departments having more than 5 employees
SELECT department_id
FROM employees
GROUP BY department_id
HAVING COUNT(*) > (
    SELECT COUNT(*)
    FROM employees
    WHERE department_id = 10
);
```

### B) **In FROM (Derived Tables)**
```sql
SELECT d.department_name, sub.avg_salary
FROM (
    SELECT department_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department_id
) sub
JOIN departments d ON sub.department_id = d.department_id;
```

### C) **In SELECT**
```sql
SELECT e.first_name,
       e.salary,
       (SELECT MAX(salary) FROM employees) AS company_max_salary
FROM employees e;
```

### D) **With EXISTS / NOT EXISTS**
```sql
-- Employees who belong to a department
SELECT first_name, last_name
FROM employees e
WHERE EXISTS (
    SELECT 1
    FROM departments d
    WHERE d.department_id = e.department_id
);

-- Employees who don’t belong to any department
SELECT first_name, last_name
FROM employees e
WHERE NOT EXISTS (
    SELECT 1
    FROM departments d
    WHERE d.department_id = e.department_id
);
```

### E) **With IN / NOT IN**
```sql
-- Employees in departments located in New York
SELECT first_name, last_name
FROM employees
WHERE department_id IN (
    SELECT department_id
    FROM departments
    WHERE location_id = 'NEW YORK'
);
```

### F) **With ANY / ALL**
```sql
-- Employees whose salary is greater than any salary in dept 10
SELECT first_name, salary
FROM employees
WHERE salary > ANY (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);

-- Employees whose salary is greater than all salaries in dept 10
SELECT first_name, salary
FROM employees
WHERE salary > ALL (
    SELECT salary
    FROM employees
    WHERE department_id = 10
);
```

---

## 4. Performance Considerations
- **Uncorrelated subqueries** are generally faster because they run once.
- **Correlated subqueries** can be expensive (executed for each outer row).  
  - PostgreSQL sometimes optimizes them by rewriting into `JOIN`s or `SEMI-JOINs`.

Example:
```sql
-- Correlated version
SELECT e1.first_name, e1.salary
FROM employees e1
WHERE e1.salary > (
    SELECT AVG(e2.salary)
    FROM employees e2
    WHERE e2.department_id = e1.department_id
);

-- Equivalent JOIN version (usually faster)
SELECT e1.first_name, e1.salary
FROM employees e1
JOIN (
    SELECT department_id, AVG(salary) AS dept_avg
    FROM employees
    GROUP BY department_id
) e2_avg
ON e1.department_id = e2_avg.department_id
WHERE e1.salary > e2_avg.dept_avg;
```

---

## 5. Common Pitfalls
- **Using `=` with a subquery that returns multiple rows** → error. Use `IN` or rewrite with `JOIN`.
- **NULL values** can make `NOT IN` behave unexpectedly. Prefer `NOT EXISTS`.
- **Performance**: Avoid correlated subqueries if a `JOIN` can achieve the same.

---

## 6. Practical Use Cases
- Filtering dynamically (`WHERE ... IN (SELECT ...)`)
- Ranking and comparisons (salary above department average)
- Checking existence (`EXISTS`)
- Encapsulating reusable logic (derived tables)
- Simplifying complex joins