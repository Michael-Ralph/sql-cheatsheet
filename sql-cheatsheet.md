# SQL Cheatsheet

## Basic Queries

### SELECT
```sql
-- Basic select
SELECT column1, column2 FROM table_name;

-- Select all columns
SELECT * FROM table_name;

-- Select with alias
SELECT column1 AS col1, column2 AS col2 FROM table_name;

-- Select distinct values
SELECT DISTINCT column1 FROM table_name;

-- Limit results
SELECT * FROM table_name LIMIT 10;
```

### WHERE
```sql
-- Basic filtering
SELECT * FROM table_name WHERE condition;

-- Common operators: =, >, <, >=, <=, <>, !=, LIKE, IN, BETWEEN
SELECT * FROM table_name WHERE column1 = 'value';
SELECT * FROM table_name WHERE column1 > 100;
SELECT * FROM table_name WHERE column1 BETWEEN 10 AND 50;
SELECT * FROM table_name WHERE column1 IN ('value1', 'value2');
SELECT * FROM table_name WHERE column1 LIKE 'pattern%';
```

### ORDER BY
```sql
-- Ascending order (default)
SELECT * FROM table_name ORDER BY column1;
SELECT * FROM table_name ORDER BY column1 ASC;

-- Descending order
SELECT * FROM table_name ORDER BY column1 DESC;

-- Multiple columns
SELECT * FROM table_name ORDER BY column1 ASC, column2 DESC;
```

### GROUP BY and HAVING
```sql
-- Group records
SELECT column1, COUNT(*) FROM table_name GROUP BY column1;

-- Filter groups
SELECT column1, COUNT(*) FROM table_name GROUP BY column1 HAVING COUNT(*) > 5;
```

## Joins

### INNER JOIN
```sql
SELECT a.column1, b.column2
FROM table1 a
INNER JOIN table2 b ON a.key = b.key;
```

### LEFT JOIN (LEFT OUTER JOIN)
```sql
SELECT a.column1, b.column2
FROM table1 a
LEFT JOIN table2 b ON a.key = b.key;
```

### RIGHT JOIN (RIGHT OUTER JOIN)
```sql
SELECT a.column1, b.column2
FROM table1 a
RIGHT JOIN table2 b ON a.key = b.key;
```

### FULL JOIN (FULL OUTER JOIN)
```sql
SELECT a.column1, b.column2
FROM table1 a
FULL JOIN table2 b ON a.key = b.key;
```

### CROSS JOIN
```sql
SELECT a.column1, b.column2
FROM table1 a
CROSS JOIN table2 b;
```

## Data Manipulation

### INSERT
```sql
-- Insert single row
INSERT INTO table_name (column1, column2) VALUES ('value1', 'value2');

-- Insert multiple rows
INSERT INTO table_name (column1, column2)
VALUES ('value1', 'value2'), ('value3', 'value4');

-- Insert from select
INSERT INTO table_name (column1, column2)
SELECT column1, column2 FROM another_table WHERE condition;
```

### UPDATE
```sql
-- Update all rows
UPDATE table_name SET column1 = 'value1';

-- Update with condition
UPDATE table_name SET column1 = 'value1', column2 = 'value2'
WHERE condition;
```

### DELETE
```sql
-- Delete all rows
DELETE FROM table_name;

-- Delete with condition
DELETE FROM table_name WHERE condition;
```

### TRUNCATE
```sql
-- Delete all rows (faster than DELETE, resets auto-increment)
TRUNCATE TABLE table_name;
```

## Data Definition

### CREATE TABLE
```sql
CREATE TABLE table_name (
    column1 datatype1 constraints,
    column2 datatype2 constraints,
    ...
    table_constraint1,
    table_constraint2
);

-- Example
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    hire_date DATE,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

### ALTER TABLE
```sql
-- Add column
ALTER TABLE table_name ADD column_name datatype constraints;

-- Drop column
ALTER TABLE table_name DROP COLUMN column_name;

-- Modify column
ALTER TABLE table_name MODIFY column_name new_datatype new_constraints;
ALTER TABLE table_name ALTER COLUMN column_name new_datatype new_constraints;

-- Rename column
ALTER TABLE table_name RENAME COLUMN old_name TO new_name;

-- Add constraint
ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint_definition;
```

### DROP TABLE
```sql
DROP TABLE table_name;
```

### CREATE INDEX
```sql
-- Create index
CREATE INDEX index_name ON table_name (column1, column2);

-- Create unique index
CREATE UNIQUE INDEX index_name ON table_name (column1);
```

## Common Functions

### Aggregate Functions
```sql
SELECT 
    COUNT(*) as count,
    SUM(column1) as total,
    AVG(column1) as average,
    MIN(column1) as minimum,
    MAX(column1) as maximum
FROM table_name;
```

### String Functions
```sql
SELECT 
    CONCAT(first_name, ' ', last_name) as full_name,
    UPPER(column1) as uppercase,
    LOWER(column1) as lowercase,
    LENGTH(column1) as string_length,
    SUBSTRING(column1, 1, 5) as extract,
    TRIM(column1) as trimmed
FROM table_name;
```

### Date Functions
```sql
SELECT 
    CURRENT_DATE() as today,
    CURRENT_TIMESTAMP() as now,
    EXTRACT(YEAR FROM date_column) as year,
    EXTRACT(MONTH FROM date_column) as month,
    EXTRACT(DAY FROM date_column) as day,
    DATE_ADD(date_column, INTERVAL 1 DAY) as tomorrow,
    DATE_SUB(date_column, INTERVAL 1 MONTH) as previous_month,
    DATEDIFF(end_date, start_date) as days_difference
FROM table_name;
```

### Mathematical Functions
```sql
SELECT 
    ABS(column1) as absolute_value,
    ROUND(column1, 2) as rounded,
    CEIL(column1) as ceiling,
    FLOOR(column1) as floor,
    POWER(column1, 2) as squared,
    SQRT(column1) as square_root
FROM table_name;
```

## Advanced Concepts

### Subqueries
```sql
-- Subquery in WHERE
SELECT * FROM table1 
WHERE column1 IN (SELECT column1 FROM table2 WHERE condition);

-- Subquery in FROM
SELECT a.column1 
FROM (SELECT * FROM table1 WHERE condition) a;

-- Correlated subquery
SELECT * FROM table1 t1
WHERE column1 > (SELECT AVG(column1) FROM table1 t2 WHERE t2.column2 = t1.column2);
```

### Common Table Expressions (CTE)
```sql
WITH cte_name AS (
    SELECT column1, column2 FROM table_name WHERE condition
)
SELECT * FROM cte_name;

-- Multiple CTEs
WITH 
    cte1 AS (SELECT * FROM table1 WHERE condition1),
    cte2 AS (SELECT * FROM table2 WHERE condition2)
SELECT * FROM cte1 JOIN cte2 ON cte1.key = cte2.key;
```

### Window Functions
```sql
-- ROW_NUMBER
SELECT column1, column2,
    ROW_NUMBER() OVER (PARTITION BY column1 ORDER BY column2) as row_num
FROM table_name;

-- RANK and DENSE_RANK
SELECT column1, column2,
    RANK() OVER (ORDER BY column2) as rank,
    DENSE_RANK() OVER (ORDER BY column2) as dense_rank
FROM table_name;

-- Aggregate window functions
SELECT column1, column2,
    SUM(column2) OVER (PARTITION BY column1) as total,
    AVG(column2) OVER (PARTITION BY column1) as average
FROM table_name;
```

### CASE Expressions
```sql
SELECT 
    column1,
    CASE
        WHEN condition1 THEN result1
        WHEN condition2 THEN result2
        ELSE result3
    END as new_column
FROM table_name;
```

### Views
```sql
-- Create view
CREATE VIEW view_name AS
SELECT column1, column2 FROM table_name WHERE condition;

-- Use view
SELECT * FROM view_name;

-- Drop view
DROP VIEW view_name;
```

## Transactions

```sql
-- Start transaction
BEGIN TRANSACTION;
-- or
START TRANSACTION;

-- Commit transaction
COMMIT;

-- Rollback transaction
ROLLBACK;

-- Savepoint
SAVEPOINT savepoint_name;
ROLLBACK TO SAVEPOINT savepoint_name;
RELEASE SAVEPOINT savepoint_name;
```

## Database Management

```sql
-- Create database
CREATE DATABASE database_name;

-- Use database
USE database_name;

-- Drop database
DROP DATABASE database_name;

-- Create user
CREATE USER 'username'@'host' IDENTIFIED BY 'password';

-- Grant privileges
GRANT privilege_name ON database_name.table_name TO 'username'@'host';

-- Revoke privileges
REVOKE privilege_name ON database_name.table_name FROM 'username'@'host';
```
