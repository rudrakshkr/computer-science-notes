# SQL & PostgreSQL — Filtering, Aggregation and GROUP BY

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
- Except for `COUNT`, aggregate functions generally return NULL when there are no non-NULL input values.

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

The simplified logical processing order for these clauses is:

```text
FROM
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

## 8. Common Interview Mistakes

- Using `WHERE COUNT(*) >= 2` instead of `HAVING COUNT(*) >= 2`.
- Forgetting `GROUP BY` when calculating an aggregate per department.
- Confusing `COUNT(*)` with `COUNT(column)`.
- Forgetting that `COUNT(DISTINCT column)` excludes NULL values.
- Selecting an ordinary column that is neither grouped nor aggregated.
- Assuming `WHERE` can filter aggregate results at the same query level.

---

## 9. Quick Revision

| Requirement | SQL concept |
|---|---|
| Count all employees | `COUNT(*)` |
| Calculate average salary | `AVG(salary)` |
| Find total salary | `SUM(salary)` |
| Calculate average salary per department | `GROUP BY department` |
| Find departments with at least five employees | `HAVING COUNT(*) >= 5` |
| Count unique departments | `COUNT(DISTINCT department)` |
| Filter employees before grouping | `WHERE` |
| Filter groups after aggregation | `HAVING` |

## Key Takeaway

**Use aggregate functions to calculate summaries, GROUP BY to calculate them for each group, WHERE to filter rows before grouping, and HAVING to filter groups after aggregation.**