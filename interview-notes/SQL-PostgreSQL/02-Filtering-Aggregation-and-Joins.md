# SQL & PostgreSQL — Filtering, Aggregation, GROUP BY and JOINs

## 1. Aggregate Functions

Aggregate functions calculate a single result from values across multiple rows.

| Function | Purpose |
|---|---|
| `COUNT()` | Counts rows or non-NULL values |
| `SUM()` | Calculates the total |
| `AVG()` | Calculates the average |
| `MIN()` | Returns the minimum value |
| `MAX()` | Returns the maximum value |

### Examples

```sql
-- Count all employees
SELECT COUNT(*)
FROM employees;

-- Calculate total salary
SELECT SUM(salary)
FROM employees;

-- Calculate average salary
SELECT AVG(salary)
FROM employees;

-- Find lowest salary
SELECT MIN(salary)
FROM employees;

-- Find highest salary
SELECT MAX(salary)
FROM employees;
```

### COUNT(*) vs COUNT(column)

```sql
COUNT(*)                   -- Counts all rows
COUNT(salary)              -- Counts rows with non-NULL salary
COUNT(DISTINCT department) -- Counts unique, non-NULL departments
```

Important:

- `COUNT(*)` counts every row, including rows containing NULL values.
- `COUNT(column)` ignores NULL values in that column.
- `COUNT(DISTINCT column)` counts unique, non-NULL values.
- `SUM`, `AVG`, `MIN`, and `MAX` ignore NULL values.
- `COUNT` returns 0 when there are no input rows to count. Other aggregate functions generally return NULL when there are no non-NULL input values.

---

## 2. GROUP BY

`GROUP BY` groups rows that share the same value or combination of values.

It is commonly used with aggregate functions to calculate results for each group.

### Example: Average salary by department

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department;
```

This produces one result row per department, containing its average salary.

### Example: Count employees per department

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

### Grouping by multiple columns

```sql
SELECT department, city, COUNT(*) AS employee_count
FROM employees
GROUP BY department, city;
```

This creates a separate group for each unique department-and-city combination.

For example, employees in Engineering in Delhi form a different group from employees in Engineering in Mumbai.

---

## 3. WHERE vs HAVING

This is a common SQL interview question.

### WHERE

`WHERE` filters individual rows before grouping.

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
WHERE salary >= 50000
GROUP BY department;
```

Only employees whose salary is at least 50000 participate in the grouping and average calculation.

### HAVING

`HAVING` filters groups after grouping and aggregation.

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

Only departments whose average salary exceeds 50000 are returned.

### Comparison

| WHERE | HAVING |
|---|---|
| Filters individual rows | Filters groups |
| Applied before grouping | Applied after grouping |
| Commonly filters column values | Commonly filters aggregate results |

### Using WHERE and HAVING together

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
WHERE salary >= 50000
GROUP BY department
HAVING AVG(salary) > 55000;
```

This:

1. Excludes employees earning less than 50000.
2. Groups the remaining employees by department.
3. Calculates each department's average salary.
4. Keeps only departments whose average exceeds 55000.

---

## 4. COUNT(DISTINCT)

Use `COUNT(DISTINCT column)` to count unique values.

```sql
SELECT COUNT(DISTINCT department)
FROM employees;
```

This counts how many different non-NULL department values exist.

Remember:

```sql
COUNT(*)                   -- All rows
COUNT(department)          -- Non-NULL department values
COUNT(DISTINCT department) -- Unique, non-NULL department values
```

---

## 5. GROUP BY Rules

When using `GROUP BY`, each selected column must generally either:

- Appear in the `GROUP BY` clause, or
- Be used inside an aggregate function.

### Valid query

```sql
SELECT department, AVG(salary)
FROM employees
GROUP BY department;
```

### Invalid in ordinary SQL

```sql
SELECT department, name, AVG(salary)
FROM employees
GROUP BY department;
```

A department can contain multiple employee names, so SQL cannot determine which single name should represent that group.

Use another query approach if individual employee details are also required.

---

## 6. Combining Aggregation and Filtering

Example requirement: Find departments that have at least two employees and an average salary above 50000.

```sql
SELECT
    department,
    COUNT(*) AS employee_count,
    AVG(salary) AS average_salary
FROM employees
GROUP BY department
HAVING COUNT(*) >= 2
   AND AVG(salary) > 50000
ORDER BY average_salary DESC;
```

This query:

1. Reads the employee records.
2. Groups employees by department.
3. Calculates each department's employee count and average salary.
4. Keeps groups satisfying both conditions.
5. Sorts the results by average salary in descending order.

---

## 7. SQL Logical Processing Order

The simplified logical processing order is:

```text
FROM (including JOIN and ON)
       ↓
WHERE
       ↓
GROUP BY
       ↓
HAVING
       ↓
SELECT
       ↓
ORDER BY
       ↓
LIMIT
```

This helps explain why aggregate conditions generally belong in `HAVING` rather than `WHERE`.

This is the logical processing order, not necessarily the physical execution order chosen by the database engine.

---

# JOINs

## 8. What Is a JOIN?

A `JOIN` combines rows from two or more tables based on a related column or matching condition.

Relational databases often store related information in separate tables to reduce unnecessary duplication.

Example tables:

**employees**

| id | name | department_id |
|---:|---|---:|
| 1 | Rahul | 10 |
| 2 | Priya | 20 |
| 3 | Aman | 10 |
| 4 | Neha | 30 |

**departments**

| id | department_name |
|---:|---|
| 10 | Engineering |
| 20 | Marketing |
| 40 | Finance |

The `employees.department_id` column relates to `departments.id`.

A JOIN allows us to retrieve employee and department information together.

---

## 9. INNER JOIN

An `INNER JOIN` returns only rows that have a matching record in both tables.

```sql
SELECT e.name, d.department_name
FROM employees AS e
INNER JOIN departments AS d
    ON e.department_id = d.id;
```

Result:

| name | department_name |
|---|---|
| Rahul | Engineering |
| Priya | Marketing |
| Aman | Engineering |

Neha is excluded because department 30 has no matching row in `departments`.

Finance is excluded because no employee belongs to department 40.

**Key idea:** Only matching pairs are returned.

In PostgreSQL, writing `JOIN` without specifying a type means `INNER JOIN`.

```sql
SELECT e.name, d.department_name
FROM employees AS e
JOIN departments AS d
    ON e.department_id = d.id;
```

This is equivalent to the first query.

---

## 10. LEFT JOIN

A `LEFT JOIN` returns every row from the left table and matching rows from the right table.

If no match exists, the right table's columns contain NULL.

```sql
SELECT e.name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.id;
```

Result:

| name | department_name |
|---|---|
| Rahul | Engineering |
| Priya | Marketing |
| Aman | Engineering |
| Neha | NULL |

Neha is included despite having no matching department.

**Key idea:** Preserve every row from the left table.

Common use cases:

- List all customers, including those without orders.
- List all employees, including those without a matching department.
- Find records that have no related records in another table.

---

## 11. RIGHT JOIN

A `RIGHT JOIN` returns every row from the right table and matching rows from the left table.

```sql
SELECT e.name, d.department_name
FROM employees AS e
RIGHT JOIN departments AS d
    ON e.department_id = d.id;
```

Result:

| name | department_name |
|---|---|
| Rahul | Engineering |
| Aman | Engineering |
| Priya | Marketing |
| NULL | Finance |

Finance is included even though no employee belongs to it.

**Key idea:** Preserve every row from the right table.

A `RIGHT JOIN` can usually be rewritten as a `LEFT JOIN` by reversing the table order. Many developers prefer this for readability.

---

## 12. FULL OUTER JOIN

A `FULL OUTER JOIN` preserves all rows from both tables.

- Matching rows are combined.
- Unmatched left-table rows contain NULL for right-table columns.
- Unmatched right-table rows contain NULL for left-table columns.

```sql
SELECT e.name, d.department_name
FROM employees AS e
FULL OUTER JOIN departments AS d
    ON e.department_id = d.id;
```

Result:

| name | department_name |
|---|---|
| Rahul | Engineering |
| Priya | Marketing |
| Aman | Engineering |
| Neha | NULL |
| NULL | Finance |

**Key idea:** Preserve everything from both tables, whether or not a match exists.

PostgreSQL supports `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, and `FULL OUTER JOIN`.

---

## 13. JOIN Types — Quick Comparison

| JOIN type | What it returns |
|---|---|
| `INNER JOIN` | Only matching rows |
| `LEFT JOIN` | All left rows and matching right rows |
| `RIGHT JOIN` | All right rows and matching left rows |
| `FULL OUTER JOIN` | All rows from both tables |

Remember:

- INNER = matches only.
- LEFT = preserve the left table.
- RIGHT = preserve the right table.
- FULL = preserve both tables.

---

## 14. The ON Clause and Table Aliases

The `ON` clause defines the condition used to match rows between tables.

```sql
SELECT e.name, d.department_name
FROM employees AS e
JOIN departments AS d
    ON e.department_id = d.id;
```

Here:

- `e` is an alias for `employees`.
- `d` is an alias for `departments`.
- `e.department_id = d.id` is the matching condition.

Aliases make queries shorter and are particularly useful when joining multiple tables or joining a table to itself.

A JOIN can also use multiple matching conditions:

```sql
SELECT *
FROM table_a AS a
JOIN table_b AS b
    ON a.id = b.id
   AND a.region = b.region;
```

Both conditions must be satisfied for a row pair to match.

---

## 15. Joining More Than Two Tables

You can chain multiple JOINs in one query.

For example, suppose we have `employees`, `departments`, and `locations`.

```sql
SELECT
    e.name,
    d.department_name,
    l.city
FROM employees AS e
JOIN departments AS d
    ON e.department_id = d.id
JOIN locations AS l
    ON d.location_id = l.id;
```

This joins employees to departments and then joins those department records to locations.

**Interview takeaway:** Multiple JOINs are common when related information is stored across several tables.

---

## 16. ON vs WHERE

This is an important interview distinction.

- `ON` defines how rows from tables match.
- `WHERE` filters the rows produced by the join.

### Example

```sql
SELECT e.name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.id
WHERE d.department_name = 'Engineering';
```

Although this query uses a `LEFT JOIN`, the `WHERE` condition removes rows whose department name is NULL.

As a result, employees without a matching department are excluded.

For this particular condition, the result behaves like an INNER JOIN filtered to Engineering employees.

### Important rule

A condition on the right table in a `WHERE` clause can remove unmatched rows preserved by a `LEFT JOIN`.

If you want to retain all employees but match only Engineering departments, put the condition in `ON` instead:

```sql
SELECT e.name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.id
   AND d.department_name = 'Engineering';
```

This preserves every employee. Employees who are not in Engineering, or have no matching department, receive NULL for the department columns.

**Key takeaway:** Condition placement matters, particularly with outer joins.

---

## 17. Finding Rows Without a Match

A common interview pattern is to find records from one table that have no matching record in another.

Use a `LEFT JOIN` followed by a NULL check on a right-table column that is non-NULL for actual matching records.

### Example: Employees without a matching department

```sql
SELECT e.name
FROM employees AS e
LEFT JOIN departments AS d
    ON e.department_id = d.id
WHERE d.id IS NULL;
```

Result:

| name |
|---|
| Neha |

The `LEFT JOIN` preserves Neha, while `WHERE d.id IS NULL` selects rows without a department match.

This pattern can be used to find:

- Customers who have never placed an order.
- Products that have never been sold.
- Employees without matching departments.
- Users who have never completed a particular activity.

**Key pattern to remember:**

```sql
SELECT a.*
FROM table_a AS a
LEFT JOIN table_b AS b
    ON a.id = b.a_id
WHERE b.a_id IS NULL;
```

Use a right-table column that cannot be NULL in a valid matching record. Otherwise, a legitimate match containing NULL could be mistaken for an unmatched row.

---

## 18. Combining JOINs with Aggregate Functions

JOINs are frequently combined with aggregate functions to calculate totals or counts for related records.

Suppose we have:

**customers**

| id | name |
|---:|---|
| 1 | Rahul |
| 2 | Priya |
| 3 | Aman |

**orders**

| id | customer_id | amount |
|---:|---:|---:|
| 101 | 1 | 500 |
| 102 | 1 | 300 |
| 103 | 2 | 700 |

Requirement: Calculate each customer's total spending, including customers who have never placed an order.

```sql
SELECT
    c.id,
    c.name,
    COALESCE(SUM(o.amount), 0) AS total_spending
FROM customers AS c
LEFT JOIN orders AS o
    ON c.id = o.customer_id
GROUP BY c.id, c.name
ORDER BY total_spending DESC;
```

Result:

| id | name | total_spending |
|---:|---|---:|
| 1 | Rahul | 800 |
| 2 | Priya | 700 |
| 3 | Aman | 0 |

Explanation:

1. `LEFT JOIN` preserves customers who have no orders.
2. `GROUP BY` groups records by customer.
3. `SUM(o.amount)` calculates each customer's total spending.
4. `COALESCE(..., 0)` replaces NULL totals with zero.
5. `ORDER BY` sorts customers by their total spending.

### COUNT(*) vs COUNT(column) after a LEFT JOIN

This is a common interview pitfall.

Suppose a customer has no orders:

```sql
SELECT
    c.id,
    COUNT(*) AS row_count,
    COUNT(o.id) AS order_count
FROM customers AS c
LEFT JOIN orders AS o
    ON c.id = o.customer_id
GROUP BY c.id;
```

For a customer with no orders:

- `COUNT(*)` returns `1` because the LEFT JOIN still produces a row for the customer.
- `COUNT(o.id)` returns `0` because `o.id` is NULL in that row.

When counting related records after a LEFT JOIN, `COUNT(right_table.primary_key)` is often the appropriate choice.

---

## 19. JOINs and Duplicate Rows

JOINs do not automatically remove duplicate-looking rows.

If one customer has three orders, joining that customer to the orders table generates three rows for that customer.

That is normal one-to-many JOIN behavior.

### A more subtle problem

Suppose a customer has three orders and two support tickets. If both tables are joined directly to the customer table, the result may contain six rows for that customer.

This can inflate aggregate calculations.

For example:

- `COUNT(order_id)` may count the same order multiple times.
- `SUM(order_amount)` may add the same order amounts multiple times.

### How to avoid this

- Understand the relationships between tables: one-to-one, one-to-many, and many-to-many.
- Aggregate related tables separately before joining when multiple one-to-many relationships would multiply rows.
- Use `COUNT(DISTINCT order_id)` when the requirement is to count unique orders and the query's structure warrants it.

Important: `COUNT(DISTINCT ...)` does not automatically fix every aggregate problem. It will not, for example, prevent an inflated `SUM(order_amount)`.

---

## 20. SELF JOIN

A SELF JOIN joins a table to itself.

It is useful when records in the same table are related to one another, such as employees and their managers.

Example `employees` table:

| id | name | manager_id |
|---:|---|---:|
| 1 | Asha | NULL |
| 2 | Rahul | 1 |
| 3 | Priya | 1 |

To display employees alongside their managers:

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees AS e
LEFT JOIN employees AS m
    ON e.manager_id = m.id;
```

Result:

| employee | manager |
|---|---|
| Asha | NULL |
| Rahul | Asha |
| Priya | Asha |

Here, `employees` is used twice:

- `e` represents the employee.
- `m` represents the manager.

The `LEFT JOIN` keeps employees who do not have a manager.

### What if we use INNER JOIN?

```sql
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees AS e
INNER JOIN employees AS m
    ON e.manager_id = m.id;
```

Asha is excluded because she has no matching manager.

**Important distinction:**

- SELF JOIN describes joining a table to itself.
- INNER JOIN, LEFT JOIN, and other JOIN types determine which rows are preserved.

A SELF JOIN is not a separate JOIN type.

---

## 21. CROSS JOIN

A `CROSS JOIN` returns every possible combination of rows from two tables.

If table A contains 3 rows and table B contains 4 rows, the result contains:

3 × 4 = 12 rows.

```sql
SELECT *
FROM colors
CROSS JOIN sizes;
```

This can be useful when generating combinations, such as every available color-and-size combination for a product.

Unlike an ordinary INNER JOIN, a CROSS JOIN does not require a matching condition.

**Warning:** A CROSS JOIN between large tables can generate an enormous result.

---

## 23. Quick Revision

| Requirement | SQL concept |
|---|---|
| Count all employees | `COUNT(*)` |
| Count non-NULL salaries | `COUNT(salary)` |
| Count unique departments | `COUNT(DISTINCT department)` |
| Calculate average salary per department | `GROUP BY department` |
| Find departments with at least five employees | `HAVING COUNT(*) >= 5` |
| Filter rows before grouping | `WHERE` |
| Filter groups after aggregation | `HAVING` |
| Return only matching rows | `INNER JOIN` |
| Preserve all left-table rows | `LEFT JOIN` |
| Preserve all right-table rows | `RIGHT JOIN` |
| Preserve rows from both tables | `FULL OUTER JOIN` |
| Find records without a related record | `LEFT JOIN` + `WHERE right_table.id IS NULL` |
| Join a table to itself | SELF JOIN |
| Generate every combination of two tables | `CROSS JOIN` |
| Count matching child records after a LEFT JOIN | `COUNT(right_table.id)` |
| Include entities without related records in results | Usually `LEFT JOIN` |
| Filter matching conditions in a join | `ON` |
| Filter rows after a join | `WHERE` |