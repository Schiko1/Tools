- superuser login: sudo -u postgres psql

- Login: psql -U sherifh -h localhost -d postgres

- CREATE DATABASE mydb;

- DROP DATABASE mydb;

- Run .sql file
	- \i  /path/to/my_script.sql

- Exit psql: CTRL + D

- clear terminal: CTRL + L

- see all tables of a database
```sql
SELECT table_schema, table_name  
FROM information_schema.tables  
WHERE table_type = 'BASE TABLE'  
AND table_schema NOT IN ('pg_catalog', 'information_schema');
```