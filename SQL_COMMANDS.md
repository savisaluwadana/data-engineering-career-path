# SQL Commands - From Beginner to Advanced

This comprehensive guide covers SQL commands from beginner to advanced levels with practical examples.

## Table of Contents
- [Beginner Level](#beginner-level)
- [Intermediate Level](#intermediate-level)
- [Advanced Level](#advanced-level)

---

## Beginner Level

### 1. SELECT - Retrieve Data

The SELECT statement is used to query data from a database.

```sql
-- Select all columns from a table
SELECT * FROM employees;

-- Select specific columns
SELECT first_name, last_name, salary FROM employees;

-- Select with alias
SELECT first_name AS "First Name", last_name AS "Last Name" FROM employees;
```

### 2. WHERE - Filter Data

Filter records based on specified conditions.

```sql
-- Simple WHERE clause
SELECT * FROM employees WHERE salary > 50000;

-- Multiple conditions with AND
SELECT * FROM employees WHERE salary > 50000 AND department = 'IT';

-- Multiple conditions with OR
SELECT * FROM employees WHERE department = 'IT' OR department = 'HR';

-- Using IN operator
SELECT * FROM employees WHERE department IN ('IT', 'HR', 'Finance');

-- Using BETWEEN
SELECT * FROM employees WHERE salary BETWEEN 40000 AND 60000;

-- Using LIKE for pattern matching
SELECT * FROM employees WHERE first_name LIKE 'J%';

-- Using NOT
SELECT * FROM employees WHERE department NOT IN ('IT', 'HR');
```

### 3. ORDER BY - Sort Results

Sort the result set in ascending or descending order.

```sql
-- Order by one column (ascending by default)
SELECT * FROM employees ORDER BY salary;

-- Order by descending
SELECT * FROM employees ORDER BY salary DESC;

-- Order by multiple columns
SELECT * FROM employees ORDER BY department ASC, salary DESC;
```

### 4. LIMIT/TOP - Limit Results

Limit the number of rows returned.

```sql
-- MySQL/PostgreSQL
SELECT * FROM employees ORDER BY salary DESC LIMIT 10;

-- SQL Server
SELECT TOP 10 * FROM employees ORDER BY salary DESC;

-- Oracle
SELECT * FROM employees WHERE ROWNUM <= 10 ORDER BY salary DESC;
```

### 5. INSERT - Add Data

Insert new records into a table.

```sql
-- Insert single row with all columns
INSERT INTO employees (employee_id, first_name, last_name, email, salary, department)
VALUES (1, 'John', 'Doe', 'john.doe@example.com', 55000, 'IT');

-- Insert single row with specific columns
INSERT INTO employees (first_name, last_name, email)
VALUES ('Jane', 'Smith', 'jane.smith@example.com');

-- Insert multiple rows
INSERT INTO employees (first_name, last_name, email, salary, department)
VALUES 
    ('Alice', 'Johnson', 'alice.j@example.com', 60000, 'HR'),
    ('Bob', 'Williams', 'bob.w@example.com', 65000, 'IT'),
    ('Carol', 'Brown', 'carol.b@example.com', 58000, 'Finance');
```

### 6. UPDATE - Modify Data

Update existing records in a table.

```sql
-- Update single column
UPDATE employees SET salary = 60000 WHERE employee_id = 1;

-- Update multiple columns
UPDATE employees 
SET salary = 65000, department = 'Senior IT'
WHERE employee_id = 1;

-- Update with calculation
UPDATE employees SET salary = salary * 1.10 WHERE department = 'IT';

-- Update all rows (be careful!)
UPDATE employees SET status = 'Active';
```

### 7. DELETE - Remove Data

Delete records from a table.

```sql
-- Delete specific rows
DELETE FROM employees WHERE employee_id = 1;

-- Delete with condition
DELETE FROM employees WHERE department = 'IT' AND salary < 40000;

-- Delete all rows (be very careful!)
DELETE FROM employees;
```

### 8. CREATE TABLE - Create Tables

Create a new table in the database.

```sql
-- Basic table creation
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    salary DECIMAL(10, 2),
    department VARCHAR(50),
    hire_date DATE
);

-- Table with constraints
CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    salary DECIMAL(10, 2) CHECK (salary > 0),
    department VARCHAR(50) DEFAULT 'General',
    hire_date DATE DEFAULT CURRENT_DATE,
    manager_id INT,
    FOREIGN KEY (manager_id) REFERENCES employees(employee_id)
);
```

### 9. ALTER TABLE - Modify Tables

Modify an existing table structure.

```sql
-- Add a column
ALTER TABLE employees ADD COLUMN phone VARCHAR(20);

-- Drop a column
ALTER TABLE employees DROP COLUMN phone;

-- Modify a column
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12, 2);

-- Rename a column
ALTER TABLE employees RENAME COLUMN email TO email_address;

-- Add constraint
ALTER TABLE employees ADD CONSTRAINT chk_salary CHECK (salary > 0);
```

### 10. DROP TABLE - Delete Tables

Delete a table from the database.

```sql
-- Drop table
DROP TABLE employees;

-- Drop table if exists
DROP TABLE IF EXISTS employees;
```

### 11. DISTINCT - Remove Duplicates

Select unique values.

```sql
-- Select distinct values
SELECT DISTINCT department FROM employees;

-- Distinct with multiple columns
SELECT DISTINCT department, salary FROM employees;
```

### 12. COUNT, SUM, AVG, MIN, MAX - Aggregate Functions

Perform calculations on data.

```sql
-- Count rows
SELECT COUNT(*) FROM employees;

-- Count non-null values
SELECT COUNT(email) FROM employees;

-- Sum
SELECT SUM(salary) FROM employees;

-- Average
SELECT AVG(salary) FROM employees;

-- Min and Max
SELECT MIN(salary) AS min_salary, MAX(salary) AS max_salary FROM employees;
```

---

## Intermediate Level

### 1. GROUP BY - Group Data

Group rows that have the same values.

```sql
-- Group by one column
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;

-- Group by with multiple columns
SELECT department, YEAR(hire_date) AS hire_year, COUNT(*) AS count
FROM employees
GROUP BY department, YEAR(hire_date);

-- Group by with aggregate functions
SELECT department, AVG(salary) AS avg_salary, MAX(salary) AS max_salary
FROM employees
GROUP BY department;
```

### 2. HAVING - Filter Groups

Filter groups based on conditions (used with GROUP BY).

```sql
-- Filter groups with HAVING
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 55000;

-- Multiple conditions in HAVING
SELECT department, COUNT(*) AS emp_count
FROM employees
GROUP BY department
HAVING COUNT(*) > 5 AND AVG(salary) > 50000;
```

### 3. INNER JOIN - Join Tables

Combine rows from two or more tables based on a related column.

```sql
-- INNER JOIN
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id;

-- Multiple joins
SELECT e.first_name, e.last_name, d.department_name, p.project_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
INNER JOIN projects p ON e.employee_id = p.employee_id;

-- Join with WHERE clause
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
INNER JOIN departments d ON e.department_id = d.department_id
WHERE d.department_name = 'IT';
```

### 4. LEFT JOIN (LEFT OUTER JOIN)

Return all records from the left table and matched records from the right table.

```sql
-- LEFT JOIN
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id;

-- Find employees without departments
SELECT e.first_name, e.last_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id
WHERE d.department_id IS NULL;
```

### 5. RIGHT JOIN (RIGHT OUTER JOIN)

Return all records from the right table and matched records from the left table.

```sql
-- RIGHT JOIN
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id;

-- Find departments without employees
SELECT d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id
WHERE e.employee_id IS NULL;
```

### 6. FULL OUTER JOIN

Return all records when there is a match in either left or right table.

```sql
-- FULL OUTER JOIN
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
FULL OUTER JOIN departments d ON e.department_id = d.department_id;

-- MySQL doesn't support FULL OUTER JOIN, use UNION
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
LEFT JOIN departments d ON e.department_id = d.department_id
UNION
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
RIGHT JOIN departments d ON e.department_id = d.department_id;
```

### 7. CROSS JOIN

Return the Cartesian product of two tables.

```sql
-- CROSS JOIN
SELECT e.first_name, d.department_name
FROM employees e
CROSS JOIN departments d;

-- Alternative syntax
SELECT e.first_name, d.department_name
FROM employees e, departments d;
```

### 8. SELF JOIN

Join a table to itself.

```sql
-- Self join to find employees and their managers
SELECT e.first_name AS employee, m.first_name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id;

-- Find employees in the same department
SELECT e1.first_name AS employee1, e2.first_name AS employee2, e1.department
FROM employees e1
INNER JOIN employees e2 ON e1.department = e2.department AND e1.employee_id < e2.employee_id;
```

### 9. UNION - Combine Results

Combine the result sets of two or more SELECT statements.

```sql
-- UNION (removes duplicates)
SELECT first_name, last_name FROM employees
UNION
SELECT first_name, last_name FROM contractors;

-- UNION ALL (keeps duplicates)
SELECT first_name, last_name FROM employees
UNION ALL
SELECT first_name, last_name FROM contractors;

-- UNION with ORDER BY
SELECT first_name, last_name, 'Employee' AS type FROM employees
UNION
SELECT first_name, last_name, 'Contractor' AS type FROM contractors
ORDER BY last_name;
```

### 10. INTERSECT - Find Common Records

Return only the rows that appear in both result sets.

```sql
-- INTERSECT
SELECT employee_id FROM employees
INTERSECT
SELECT employee_id FROM project_members;

-- MySQL alternative using JOIN
SELECT DISTINCT e.employee_id
FROM employees e
INNER JOIN project_members pm ON e.employee_id = pm.employee_id;
```

### 11. EXCEPT/MINUS - Find Differences

Return rows from the first query that don't appear in the second query.

```sql
-- EXCEPT (SQL Server, PostgreSQL)
SELECT employee_id FROM employees
EXCEPT
SELECT employee_id FROM project_members;

-- MINUS (Oracle)
SELECT employee_id FROM employees
MINUS
SELECT employee_id FROM project_members;

-- MySQL alternative
SELECT e.employee_id
FROM employees e
LEFT JOIN project_members pm ON e.employee_id = pm.employee_id
WHERE pm.employee_id IS NULL;
```

### 12. Subqueries

A query nested inside another query.

```sql
-- Subquery in WHERE clause
SELECT first_name, last_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Subquery in FROM clause
SELECT dept, avg_salary
FROM (
    SELECT department AS dept, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY department
) AS dept_avg
WHERE avg_salary > 60000;

-- Subquery with IN
SELECT first_name, last_name
FROM employees
WHERE department_id IN (
    SELECT department_id FROM departments WHERE location = 'New York'
);

-- Correlated subquery
SELECT e1.first_name, e1.last_name, e1.salary
FROM employees e1
WHERE salary > (
    SELECT AVG(salary)
    FROM employees e2
    WHERE e1.department = e2.department
);
```

### 13. EXISTS - Check for Existence

Check if a subquery returns any rows.

```sql
-- EXISTS
SELECT d.department_name
FROM departments d
WHERE EXISTS (
    SELECT 1 FROM employees e WHERE e.department_id = d.department_id
);

-- NOT EXISTS
SELECT d.department_name
FROM departments d
WHERE NOT EXISTS (
    SELECT 1 FROM employees e WHERE e.department_id = d.department_id
);
```

### 14. CASE - Conditional Logic

Add conditional logic to queries.

```sql
-- Simple CASE
SELECT first_name, last_name, salary,
    CASE
        WHEN salary < 40000 THEN 'Low'
        WHEN salary BETWEEN 40000 AND 60000 THEN 'Medium'
        ELSE 'High'
    END AS salary_category
FROM employees;

-- CASE with aggregate
SELECT department,
    SUM(CASE WHEN salary > 60000 THEN 1 ELSE 0 END) AS high_earners,
    SUM(CASE WHEN salary <= 60000 THEN 1 ELSE 0 END) AS normal_earners
FROM employees
GROUP BY department;
```

### 15. NULL Handling

Work with NULL values.

```sql
-- IS NULL
SELECT * FROM employees WHERE manager_id IS NULL;

-- IS NOT NULL
SELECT * FROM employees WHERE email IS NOT NULL;

-- COALESCE (return first non-null value)
SELECT first_name, last_name, COALESCE(phone, email, 'No contact') AS contact
FROM employees;

-- NULLIF (return NULL if values are equal)
SELECT first_name, NULLIF(department, 'Unknown') AS department
FROM employees;

-- IFNULL/ISNULL (MySQL/SQL Server)
SELECT first_name, IFNULL(bonus, 0) AS bonus FROM employees;
```

### 16. String Functions

Manipulate string data.

```sql
-- CONCAT
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;

-- UPPER and LOWER
SELECT UPPER(first_name), LOWER(last_name) FROM employees;

-- SUBSTRING
SELECT SUBSTRING(email, 1, 5) AS email_prefix FROM employees;

-- LENGTH/LEN
SELECT first_name, LENGTH(first_name) AS name_length FROM employees;

-- TRIM, LTRIM, RTRIM
SELECT TRIM(first_name) FROM employees;

-- REPLACE
SELECT REPLACE(email, '@old.com', '@new.com') AS new_email FROM employees;
```

### 17. Date Functions

Work with date and time data.

```sql
-- Current date/time
SELECT CURRENT_DATE, CURRENT_TIME, CURRENT_TIMESTAMP;

-- Extract parts
SELECT YEAR(hire_date), MONTH(hire_date), DAY(hire_date) FROM employees;

-- Date arithmetic
SELECT hire_date, DATE_ADD(hire_date, INTERVAL 1 YEAR) AS anniversary FROM employees;

-- Date difference
SELECT DATEDIFF(CURRENT_DATE, hire_date) AS days_employed FROM employees;

-- Format date
SELECT DATE_FORMAT(hire_date, '%Y-%m-%d') AS formatted_date FROM employees;
```

### 18. CREATE INDEX - Improve Query Performance

Create indexes to speed up queries.

```sql
-- Create simple index
CREATE INDEX idx_employee_lastname ON employees(last_name);

-- Create composite index
CREATE INDEX idx_employee_dept_salary ON employees(department, salary);

-- Create unique index
CREATE UNIQUE INDEX idx_employee_email ON employees(email);

-- Drop index
DROP INDEX idx_employee_lastname ON employees;
```

### 19. Views - Virtual Tables

Create reusable query definitions.

```sql
-- Create view
CREATE VIEW high_earners AS
SELECT first_name, last_name, salary, department
FROM employees
WHERE salary > 70000;

-- Use view
SELECT * FROM high_earners;

-- Create or replace view
CREATE OR REPLACE VIEW employee_summary AS
SELECT department, COUNT(*) AS emp_count, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;

-- Drop view
DROP VIEW high_earners;
```

### 20. Transactions

Ensure data integrity with ACID properties.

```sql
-- Basic transaction
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT;

-- Transaction with rollback
BEGIN TRANSACTION;
UPDATE employees SET salary = salary * 1.1 WHERE department = 'IT';
-- Check if update is correct
ROLLBACK; -- or COMMIT if correct

-- Savepoint
BEGIN TRANSACTION;
UPDATE employees SET salary = salary * 1.05;
SAVEPOINT sp1;
UPDATE employees SET bonus = 1000 WHERE department = 'Sales';
ROLLBACK TO sp1; -- Rollback only the bonus update
COMMIT;
```

---

## Advanced Level

### 1. Window Functions (Analytic Functions)

Perform calculations across a set of rows related to the current row.

```sql
-- ROW_NUMBER
SELECT first_name, last_name, department, salary,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) AS row_num
FROM employees;

-- RANK and DENSE_RANK
SELECT first_name, last_name, department, salary,
    RANK() OVER (ORDER BY salary DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
FROM employees;

-- NTILE (divide into groups)
SELECT first_name, last_name, salary,
    NTILE(4) OVER (ORDER BY salary) AS quartile
FROM employees;

-- LAG and LEAD (access previous/next row)
SELECT first_name, hire_date,
    LAG(hire_date) OVER (ORDER BY hire_date) AS previous_hire,
    LEAD(hire_date) OVER (ORDER BY hire_date) AS next_hire
FROM employees;

-- FIRST_VALUE and LAST_VALUE
SELECT first_name, department, salary,
    FIRST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary) AS min_dept_salary,
    LAST_VALUE(salary) OVER (PARTITION BY department ORDER BY salary 
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS max_dept_salary
FROM employees;

-- Running total
SELECT first_name, hire_date, salary,
    SUM(salary) OVER (ORDER BY hire_date) AS running_total
FROM employees;

-- Moving average
SELECT first_name, hire_date, salary,
    AVG(salary) OVER (ORDER BY hire_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg
FROM employees;
```

### 2. Common Table Expressions (CTEs)

Create temporary named result sets.

```sql
-- Simple CTE
WITH high_earners AS (
    SELECT * FROM employees WHERE salary > 70000
)
SELECT department, COUNT(*) AS high_earner_count
FROM high_earners
GROUP BY department;

-- Multiple CTEs
WITH 
    dept_avg AS (
        SELECT department, AVG(salary) AS avg_salary
        FROM employees
        GROUP BY department
    ),
    dept_max AS (
        SELECT department, MAX(salary) AS max_salary
        FROM employees
        GROUP BY department
    )
SELECT da.department, da.avg_salary, dm.max_salary
FROM dept_avg da
JOIN dept_max dm ON da.department = dm.department;

-- Recursive CTE (employee hierarchy)
WITH RECURSIVE employee_hierarchy AS (
    -- Anchor member
    SELECT employee_id, first_name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Recursive member
    SELECT e.employee_id, e.first_name, e.manager_id, eh.level + 1
    FROM employees e
    JOIN employee_hierarchy eh ON e.manager_id = eh.employee_id
)
SELECT * FROM employee_hierarchy ORDER BY level, employee_id;

-- Recursive CTE (generate numbers)
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 100
)
SELECT * FROM numbers;
```

### 3. Pivot and Unpivot

Transform rows to columns and vice versa.

```sql
-- PIVOT (convert rows to columns)
SELECT *
FROM (
    SELECT department, YEAR(hire_date) AS year, salary
    FROM employees
) AS source_data
PIVOT (
    AVG(salary)
    FOR year IN ([2020], [2021], [2022], [2023])
) AS pivot_table;

-- Manual pivot with CASE
SELECT department,
    SUM(CASE WHEN YEAR(hire_date) = 2020 THEN salary ELSE 0 END) AS year_2020,
    SUM(CASE WHEN YEAR(hire_date) = 2021 THEN salary ELSE 0 END) AS year_2021,
    SUM(CASE WHEN YEAR(hire_date) = 2022 THEN salary ELSE 0 END) AS year_2022
FROM employees
GROUP BY department;

-- UNPIVOT (convert columns to rows)
SELECT department, year, salary
FROM salary_by_year
UNPIVOT (
    salary FOR year IN (year_2020, year_2021, year_2022)
) AS unpivot_table;
```

### 4. Advanced Joins and Set Operations

```sql
-- Lateral join (PostgreSQL) / CROSS APPLY (SQL Server)
SELECT e.first_name, e.last_name, recent_projects.project_name
FROM employees e
CROSS APPLY (
    SELECT TOP 3 project_name, start_date
    FROM projects p
    WHERE p.employee_id = e.employee_id
    ORDER BY start_date DESC
) AS recent_projects;

-- Anti-join pattern (find unmatched records)
SELECT e.*
FROM employees e
LEFT JOIN project_assignments pa ON e.employee_id = pa.employee_id
WHERE pa.employee_id IS NULL;

-- Semi-join pattern
SELECT e.*
FROM employees e
WHERE EXISTS (
    SELECT 1 FROM project_assignments pa
    WHERE pa.employee_id = e.employee_id AND pa.status = 'Active'
);
```

### 5. Advanced Subqueries

```sql
-- Scalar subquery (returns single value)
SELECT first_name, last_name, salary,
    (SELECT AVG(salary) FROM employees) AS company_avg,
    salary - (SELECT AVG(salary) FROM employees) AS diff_from_avg
FROM employees;

-- Derived table
SELECT dept, max_salary
FROM (
    SELECT department AS dept, MAX(salary) AS max_salary
    FROM employees
    GROUP BY department
) AS dept_salaries
WHERE max_salary > 80000;

-- Lateral derived table (correlated)
SELECT e.first_name, e.last_name, top_colleagues.colleague_name
FROM employees e
CROSS APPLY (
    SELECT first_name AS colleague_name
    FROM employees e2
    WHERE e2.department = e.department AND e2.employee_id != e.employee_id
    ORDER BY salary DESC
    LIMIT 3
) AS top_colleagues;
```

### 6. Partitioning and Bucketing

```sql
-- Create partitioned table (PostgreSQL)
CREATE TABLE sales (
    sale_id INT,
    sale_date DATE,
    amount DECIMAL(10, 2)
) PARTITION BY RANGE (sale_date);

CREATE TABLE sales_2023 PARTITION OF sales
FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

CREATE TABLE sales_2024 PARTITION OF sales
FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');

-- Create partitioned table (MySQL)
CREATE TABLE sales (
    sale_id INT,
    sale_date DATE,
    amount DECIMAL(10, 2)
)
PARTITION BY RANGE (YEAR(sale_date)) (
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION p_future VALUES LESS THAN MAXVALUE
);
```

### 7. JSON Operations

Work with JSON data in SQL.

```sql
-- PostgreSQL JSON operations
SELECT data->>'name' AS name, data->>'age' AS age
FROM user_data;

-- Extract nested JSON
SELECT data->'address'->>'city' AS city
FROM user_data;

-- JSON array operations
SELECT jsonb_array_elements(data->'hobbies') AS hobby
FROM user_data;

-- Build JSON
SELECT json_build_object(
    'name', first_name,
    'email', email,
    'salary', salary
) AS employee_json
FROM employees;

-- MySQL JSON operations
SELECT JSON_EXTRACT(data, '$.name') AS name
FROM user_data;

SELECT JSON_UNQUOTE(JSON_EXTRACT(data, '$.address.city')) AS city
FROM user_data;
```

### 8. Full-Text Search

Search text efficiently.

```sql
-- PostgreSQL full-text search
SELECT * FROM articles
WHERE to_tsvector('english', content) @@ to_tsquery('english', 'database & performance');

-- Create text search index
CREATE INDEX idx_articles_fts ON articles 
USING GIN (to_tsvector('english', content));

-- MySQL full-text search
SELECT * FROM articles
WHERE MATCH(title, content) AGAINST('database performance' IN NATURAL LANGUAGE MODE);

-- Boolean mode
SELECT * FROM articles
WHERE MATCH(title, content) AGAINST('+database -mysql' IN BOOLEAN MODE);
```

### 9. Stored Procedures

Create reusable SQL code blocks.

```sql
-- Create stored procedure
DELIMITER //
CREATE PROCEDURE GetEmployeesByDept(IN dept_name VARCHAR(50))
BEGIN
    SELECT first_name, last_name, salary
    FROM employees
    WHERE department = dept_name
    ORDER BY salary DESC;
END //
DELIMITER ;

-- Call procedure
CALL GetEmployeesByDept('IT');

-- Procedure with output parameter
DELIMITER //
CREATE PROCEDURE GetDepartmentStats(
    IN dept_name VARCHAR(50),
    OUT emp_count INT,
    OUT avg_salary DECIMAL(10,2)
)
BEGIN
    SELECT COUNT(*), AVG(salary)
    INTO emp_count, avg_salary
    FROM employees
    WHERE department = dept_name;
END //
DELIMITER ;

-- Call with output
CALL GetDepartmentStats('IT', @count, @avg);
SELECT @count, @avg;

-- Procedure with transaction
DELIMITER //
CREATE PROCEDURE TransferFunds(
    IN from_account INT,
    IN to_account INT,
    IN amount DECIMAL(10,2)
)
BEGIN
    DECLARE exit handler FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SELECT 'Transaction failed' AS message;
    END;
    
    START TRANSACTION;
    UPDATE accounts SET balance = balance - amount WHERE account_id = from_account;
    UPDATE accounts SET balance = balance + amount WHERE account_id = to_account;
    COMMIT;
    SELECT 'Transaction successful' AS message;
END //
DELIMITER ;
```

### 10. Functions

Create custom functions.

```sql
-- Scalar function
DELIMITER //
CREATE FUNCTION CalculateBonus(salary DECIMAL(10,2))
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN salary * 0.1;
END //
DELIMITER ;

-- Use function
SELECT first_name, salary, CalculateBonus(salary) AS bonus
FROM employees;

-- Table-valued function (SQL Server)
CREATE FUNCTION GetDepartmentEmployees(@dept VARCHAR(50))
RETURNS TABLE
AS
RETURN (
    SELECT first_name, last_name, salary
    FROM employees
    WHERE department = @dept
);

-- Use table-valued function
SELECT * FROM GetDepartmentEmployees('IT');
```

### 11. Triggers

Automatically execute code in response to events.

```sql
-- BEFORE INSERT trigger
DELIMITER //
CREATE TRIGGER before_employee_insert
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
    SET NEW.email = LOWER(NEW.email);
    SET NEW.created_at = NOW();
END //
DELIMITER ;

-- AFTER UPDATE trigger
DELIMITER //
CREATE TRIGGER after_salary_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    IF NEW.salary != OLD.salary THEN
        INSERT INTO salary_history (employee_id, old_salary, new_salary, change_date)
        VALUES (NEW.employee_id, OLD.salary, NEW.salary, NOW());
    END IF;
END //
DELIMITER ;

-- INSTEAD OF trigger (for views)
CREATE TRIGGER instead_of_delete_view
INSTEAD OF DELETE ON employee_view
FOR EACH ROW
BEGIN
    UPDATE employees SET status = 'Inactive' WHERE employee_id = OLD.employee_id;
END;
```

### 12. Dynamic SQL

Build and execute SQL statements dynamically.

```sql
-- Prepared statement (MySQL)
SET @table_name = 'employees';
SET @sql = CONCAT('SELECT * FROM ', @table_name, ' WHERE salary > ?');
PREPARE stmt FROM @sql;
SET @min_salary = 50000;
EXECUTE stmt USING @min_salary;
DEALLOCATE PREPARE stmt;

-- SQL Server dynamic SQL
DECLARE @TableName NVARCHAR(50) = 'employees';
DECLARE @SQL NVARCHAR(MAX);
SET @SQL = 'SELECT * FROM ' + QUOTENAME(@TableName) + ' WHERE salary > @MinSalary';
EXEC sp_executesql @SQL, N'@MinSalary DECIMAL(10,2)', @MinSalary = 50000;
```

### 13. Performance Optimization

```sql
-- EXPLAIN to analyze query execution
EXPLAIN SELECT e.first_name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
WHERE e.salary > 50000;

-- ANALYZE to gather statistics
ANALYZE TABLE employees;

-- Force index usage
SELECT * FROM employees FORCE INDEX (idx_salary) WHERE salary > 50000;

-- Query hints (SQL Server)
SELECT * FROM employees WITH (INDEX(idx_salary))
WHERE salary > 50000;

-- Covering index
CREATE INDEX idx_covering ON employees(department, salary) INCLUDE (first_name, last_name);
```

### 14. Advanced Constraints

```sql
-- Check constraint with function
ALTER TABLE employees
ADD CONSTRAINT chk_email_format 
CHECK (email LIKE '%@%.%');

-- Unique constraint on multiple columns
ALTER TABLE employees
ADD CONSTRAINT uq_name_dept UNIQUE (first_name, last_name, department);

-- Deferred constraints (PostgreSQL)
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    CONSTRAINT fk_customer FOREIGN KEY (customer_id) 
        REFERENCES customers(customer_id) DEFERRABLE INITIALLY DEFERRED
);

-- Exclusion constraint (PostgreSQL)
CREATE TABLE room_reservations (
    room_id INT,
    reservation_time TSRANGE,
    EXCLUDE USING GIST (room_id WITH =, reservation_time WITH &&)
);
```

### 15. Data Warehousing Queries

```sql
-- Slowly Changing Dimension Type 2
INSERT INTO dim_customer (
    customer_id, name, address, effective_date, expiry_date, is_current
)
SELECT customer_id, name, address, CURRENT_DATE, '9999-12-31', TRUE
FROM staging_customers s
WHERE NOT EXISTS (
    SELECT 1 FROM dim_customer d 
    WHERE d.customer_id = s.customer_id AND d.is_current = TRUE
);

-- Update expired records
UPDATE dim_customer d
SET expiry_date = CURRENT_DATE - INTERVAL '1 day', is_current = FALSE
WHERE d.is_current = TRUE
AND EXISTS (
    SELECT 1 FROM staging_customers s
    WHERE s.customer_id = d.customer_id 
    AND (s.name != d.name OR s.address != d.address)
);

-- Star schema query
SELECT 
    d.date_name,
    p.product_name,
    c.customer_name,
    SUM(f.amount) AS total_sales,
    COUNT(*) AS transaction_count
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_product p ON f.product_key = p.product_key
JOIN dim_customer c ON f.customer_key = c.customer_key
WHERE d.year = 2024
GROUP BY d.date_name, p.product_name, c.customer_name;
```

### 16. Graph Queries (Recursive CTEs)

```sql
-- Find all paths in a graph
WITH RECURSIVE paths AS (
    -- Start nodes
    SELECT node_id, node_id AS path, 0 AS depth
    FROM nodes
    WHERE parent_id IS NULL
    
    UNION ALL
    
    -- Recursive step
    SELECT n.node_id, CONCAT(p.path, '->', n.node_id), p.depth + 1
    FROM nodes n
    JOIN paths p ON n.parent_id = p.node_id
    WHERE p.depth < 10 -- Prevent infinite loops
)
SELECT * FROM paths;

-- Find shortest path
WITH RECURSIVE shortest_path AS (
    SELECT node_id, target_id, distance, ARRAY[node_id] AS path
    FROM edges
    WHERE node_id = 1
    
    UNION ALL
    
    SELECT e.node_id, e.target_id, sp.distance + e.distance,
           sp.path || e.target_id
    FROM edges e
    JOIN shortest_path sp ON e.node_id = sp.target_id
    WHERE NOT e.target_id = ANY(sp.path) -- Avoid cycles
)
SELECT path, distance
FROM shortest_path
WHERE target_id = 10
ORDER BY distance
LIMIT 1;
```

### 17. Temporal Tables (Time-Travel Queries)

```sql
-- SQL Server temporal table
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    salary DECIMAL(10,2),
    valid_from DATETIME2 GENERATED ALWAYS AS ROW START,
    valid_to DATETIME2 GENERATED ALWAYS AS ROW END,
    PERIOD FOR SYSTEM_TIME (valid_from, valid_to)
)
WITH (SYSTEM_VERSIONING = ON);

-- Query historical data
SELECT * FROM employees
FOR SYSTEM_TIME AS OF '2023-01-01';

SELECT * FROM employees
FOR SYSTEM_TIME BETWEEN '2023-01-01' AND '2023-12-31';

-- PostgreSQL temporal query with timestamp columns
SELECT * FROM employees
WHERE '2023-06-01' BETWEEN valid_from AND valid_to;
```

### 18. Advanced Analytical Queries

```sql
-- Cohort analysis
SELECT 
    DATE_TRUNC('month', first_purchase_date) AS cohort_month,
    DATE_TRUNC('month', purchase_date) AS purchase_month,
    COUNT(DISTINCT customer_id) AS customers,
    SUM(amount) AS revenue
FROM (
    SELECT 
        customer_id,
        purchase_date,
        MIN(purchase_date) OVER (PARTITION BY customer_id) AS first_purchase_date,
        amount
    FROM purchases
) cohort_data
GROUP BY cohort_month, purchase_month
ORDER BY cohort_month, purchase_month;

-- Funnel analysis
SELECT 
    step,
    COUNT(DISTINCT user_id) AS users,
    COUNT(DISTINCT user_id) * 100.0 / FIRST_VALUE(COUNT(DISTINCT user_id)) 
        OVER (ORDER BY step) AS conversion_rate
FROM (
    SELECT user_id, 1 AS step FROM page_views WHERE page = 'home'
    UNION ALL
    SELECT user_id, 2 AS step FROM page_views WHERE page = 'product'
    UNION ALL
    SELECT user_id, 3 AS step FROM page_views WHERE page = 'cart'
    UNION ALL
    SELECT user_id, 4 AS step FROM purchases
) funnel
GROUP BY step
ORDER BY step;

-- Running difference
SELECT 
    sale_date,
    amount,
    amount - LAG(amount) OVER (ORDER BY sale_date) AS daily_change,
    (amount - LAG(amount) OVER (ORDER BY sale_date)) * 100.0 / 
        LAG(amount) OVER (ORDER BY sale_date) AS pct_change
FROM daily_sales;

-- Cumulative distinct count
SELECT 
    date,
    COUNT(DISTINCT customer_id) AS daily_customers,
    (SELECT COUNT(DISTINCT customer_id) 
     FROM purchases p2 
     WHERE p2.date <= p1.date) AS cumulative_customers
FROM purchases p1
GROUP BY date;
```

### 19. Working with Arrays and Aggregates

```sql
-- PostgreSQL array operations
SELECT ARRAY_AGG(first_name ORDER BY salary DESC) AS employee_names
FROM employees
WHERE department = 'IT';

-- String aggregation
SELECT department, STRING_AGG(first_name, ', ' ORDER BY first_name) AS employees
FROM employees
GROUP BY department;

-- Array contains
SELECT * FROM products
WHERE categories @> ARRAY['electronics'];

-- Unnest array
SELECT unnest(ARRAY[1,2,3,4,5]) AS number;

-- JSON aggregation
SELECT department,
    JSON_AGG(
        JSON_BUILD_OBJECT(
            'name', first_name,
            'salary', salary
        )
    ) AS employees
FROM employees
GROUP BY department;
```

### 20. Advanced Window Frame Clauses

```sql
-- Custom frame specification
SELECT 
    sale_date,
    amount,
    AVG(amount) OVER (
        ORDER BY sale_date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS moving_avg_7day,
    SUM(amount) OVER (
        ORDER BY sale_date
        RANGE BETWEEN INTERVAL '30' DAY PRECEDING AND CURRENT ROW
    ) AS sum_last_30days
FROM sales;

-- Percentage of total
SELECT 
    department,
    salary,
    salary * 100.0 / SUM(salary) OVER () AS pct_of_total,
    salary * 100.0 / SUM(salary) OVER (PARTITION BY department) AS pct_of_dept
FROM employees;

-- Median using percentile
SELECT 
    department,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) AS median_salary,
    PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY salary) AS q1,
    PERCENTILE_CONT(0.75) WITHIN GROUP (ORDER BY salary) AS q3
FROM employees
GROUP BY department;
```

---

## Best Practices

1. **Always use meaningful aliases** for better readability
2. **Use explicit JOIN syntax** instead of comma-separated tables
3. **Include WHERE clauses** to filter early and improve performance
4. **Create indexes** on columns used in WHERE, JOIN, and ORDER BY
5. **Use transactions** for operations that modify multiple rows
6. **Avoid SELECT *** in production code; specify columns explicitly
7. **Use EXPLAIN** to understand query execution plans
8. **Normalize data** appropriately but consider denormalization for read-heavy workloads
9. **Use prepared statements** to prevent SQL injection
10. **Regular backups** and test restore procedures

---

## Summary

This guide covers SQL commands from basic SELECT statements to advanced topics like window functions, CTEs, and performance optimization. Practice these commands regularly to build proficiency in SQL and database management.
