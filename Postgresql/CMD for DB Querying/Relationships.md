- PRIMARY KEY: uniquely identifies each row in a table
- foreign key(REFERENCES): references the primary key in another table

```postgresql
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    name TEXT NOT NULL
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(customer_id),
    order_date DATE NOT NULL
);
```

- 1:1 Relationship
	- Each row in one table corresponds to at most one row in another table
```postgresql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    username TEXT UNIQUE
);

CREATE TABLE user_profiles (
    profile_id SERIAL PRIMARY KEY,
    user_id INT UNIQUE REFERENCES users(user_id),
    bio TEXT
);
```

- 1:n relationship
	- row in one table can relate to multiple rows in another table
	- implementd by placing a foreign key in the many side

- m:n relationship
	- a row in one table can relate to multiple rows in another, and vice versa
		- Uses junction table

```postgresql
CREATE TABLE students (
	student_id SERIAL PRIMARY KEY,
	name TEXT
);

CREATE TABLE courses (
	course_id SERIAL PRIMARY KEY,
	title TEXT
);

-- Junction table
CREATE TABLE enrollments (
	student_id INT REFERENCES students(student_id),
	course_id INT REFENCES courses(course_id)
	PRIMARY KEY (student_id, course_id)
)
```

- Referential Actions
	- `ON DELETE CASCADE`: Delete child rows if parent is deleted.
	- `ON DELETE SET NULL`: Set foreign key to `NULL` if parent is deleted.
	- `ON DELETE RESTRICT`: Prevent deletion if related rows exist.
	- `ON UPDATE CASCADE`: Update child rows if parent key changes.

- Querying Relationships
```postgresql
SELECT c.name, o.order_id, o.order_date
FROM customers c
JOIN orders o ON c.customer_id = o.customer_id;
```