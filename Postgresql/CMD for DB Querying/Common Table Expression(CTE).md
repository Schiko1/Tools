- is temporary, named result set that you can reference within a single `SELECT`, `INSERT`, `UPDATE`, or `DELETE`
- uses `WITH` clause

```postgresql
WITH cte_name AS (
    SELECT ...
)
SELECT * FROM cte_name;
```

```postgresql
WITH revenue_cte AS (
    SELECT product,
           SUM(quantity * price) AS total_revenue
    FROM sales
    GROUP BY product
)
SELECT *
FROM revenue_cte
WHERE total_revenue > 30;
```

## Multiple CTEs

```postgresql
WITH cte1 AS (
    SELECT product, SUM(quantity) AS total_qty
    FROM sales
    GROUP BY product
),
cte2 AS (
    SELECT product, SUM(quantity * price) AS total_revenue
    FROM sales
    GROUP BY product
)
SELECT c1.product, c1.total_qty, c2.total_revenue
FROM cte1 c1
JOIN cte2 c2 ON c1.product = c2.product;
```

## Recursive CTEs
- repeatedly reference itself until a condition is met

```postgresql
WITH RECURSIVE cte_name AS (
    -- Base case (anchor member)
    SELECT ...
    UNION ALL
    -- Recursive case (recursive member)
    SELECT ...
    FROM cte_name
    WHERE ...
)
SELECT * FROM cte_name;
```

```postgresql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1
    FROM numbers
    WHERE n < 10
)
SELECT * FROM numbers;
```

```markdown
n
--
1
2
3
...
10
```
### Traversing a hierarchy
```postgresql
WITH RECURSIVE employee_hierarchy AS (
    SELECT id, name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL  -- CEO

    UNION ALL

    SELECT e.id, e.name, e.manager_id, eh.level + 1
    FROM employees e
    INNER JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy;
```

## Uses
- Readable queries: Break down a complex query into logical steps
- Recursive queries: Handle hierarchical or graph-like data
- Reusability within a query: Use the same logic multiple times without rewriting
- Data transformations before final selection