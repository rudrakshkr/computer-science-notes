# SQL & PostgreSQL — 01 SQL Fundamentals

## 1. SQL, Database and PostgreSQL

- **SQL (Structured Query Language):** Language used to interact with relational databases.
- **Database:** An organized collection of data.
- **DBMS:** Software used to store, retrieve, update, and manage data.
- **PostgreSQL:** An open-source relational database management system (RDBMS) that uses SQL.

Other relational database systems include MySQL, SQLite, and Microsoft SQL Server.

## 2. Tables, Rows and Columns

- **Table:** Stores related data.
- **Row:** Represents one record.
- **Column:** Represents an attribute of a record.

Example:

| id | name | department | salary |
|---:|---|---|---:|
| 1 | Aman | Engineering | 60000 |
| 2 | Priya | HR | 45000 |
| 3 | Rahul | Engineering | 80000 |

## 3. Common SQL Data Types

| Data type | Purpose |
|---|---|
| `INTEGER` | Whole numbers |
| `VARCHAR(n)` | Variable-length text with a maximum length |
| `TEXT` | Variable-length text without a specified limit |
| `BOOLEAN` | `TRUE` or `FALSE` |
| `DATE` | Calendar date |
| `TIMESTAMP` | Date and time |
| `NUMERIC(p,s)` | Exact decimal values, such as monetary amounts |
| `UUID` | Universally unique identifiers |
| `JSONB` | JSON data stored in a format optimized for processing |

## 4. SQL Command Categories

| Category | Full form | Common commands |
|---|---|---|
| DDL | Data Definition Language | `CREATE`, `ALTER`, `DROP` |
| DML | Data Manipulation Language | `INSERT`, `UPDATE`, `DELETE` |
| DQL | Data Query Language | `SELECT` |
| DCL | Data Control Language | `GRANT`, `REVOKE` |
| TCL | Transaction Control Language | `COMMIT`, `ROLLBACK` |

## 5. CRUD Operations

CRUD stands for **Create, Read, Update, Delete**.

### Create — Insert a record

```sql
INSERT INTO users (name, email)
VALUES ('Aman', 'aman@example.com');
```

### Read — Retrieve records

```sql
SELECT *
FROM users;
```

### Update — Modify a record

```sql
UPDATE users
SET name = 'Aman Sharma'
WHERE id = 1;
```

### Delete — Remove a record

```sql
DELETE FROM users
WHERE id = 1;
```

**Important:** `UPDATE` or `DELETE` without a `WHERE` condition can affect every row in the table.

---

## 6. SELECT — Retrieve Columns

`SELECT` specifies which columns to retrieve.

```sql
SELECT name, salary
FROM employees;
```

Use `*` to retrieve all columns:

```sql
SELECT *
FROM employees;
```

In production queries, selecting only the required columns is often preferable.

## 7. WHERE — Filter Rows

`WHERE` selects rows that satisfy a condition.

```sql
SELECT *
FROM employees
WHERE salary > 50000;
```

### Common comparison operators

| Operator | Meaning |
|---|---|
| `=` | Equal to |
| `!=` or `<>` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

### Logical operators

- `AND`: Both conditions must be true.
- `OR`: At least one condition must be true.
- `NOT`: Negates a condition.

Example:

```sql
SELECT *
FROM employees
WHERE department = 'Engineering'
  AND salary >= 60000;
```

## 8. IN — Match a List of Values

`IN` checks whether a value matches any value in a specified list.

```sql
SELECT *
FROM employees
WHERE department IN ('HR', 'Marketing');
```

This returns employees from either department.

## 9. BETWEEN — Filter a Range

`BETWEEN` checks whether a value falls within an inclusive range.

```sql
SELECT *
FROM employees
WHERE salary BETWEEN 45000 AND 60000;
```

This includes salaries of both `45000` and `60000`.

**Interview point:** `BETWEEN` includes both endpoints.

## 10. ORDER BY — Sort Results

`ORDER BY` sorts query results.

```sql
SELECT name, salary
FROM employees
ORDER BY salary DESC;
```

- `ASC`: Ascending order; the default.
- `DESC`: Descending order.

Sorting by multiple columns:

```sql
SELECT *
FROM employees
ORDER BY department ASC, salary DESC;
```

This sorts departments alphabetically and employees within each department by descending salary.

**Important:** Without `ORDER BY`, SQL does not guarantee the order of returned rows.

## 11. LIMIT — Restrict the Number of Rows

`LIMIT` specifies the maximum number of rows returned.

```sql
SELECT *
FROM employees
ORDER BY salary DESC
LIMIT 2;
```

Returns the two highest-paid employees.

Combining `ORDER BY` and `LIMIT` is useful for queries such as finding the highest-paid employees or top-ranked records.

## 12. DISTINCT — Remove Duplicate Results

`DISTINCT` removes duplicate rows from the selected columns.

```sql
SELECT DISTINCT department
FROM employees;
```

This returns each department only once.

With multiple columns:

```sql
SELECT DISTINCT department, salary
FROM employees;
```

This returns unique department–salary combinations, not just unique departments.

## 13. NULL — Missing or Unknown Values

`NULL` represents a missing or unknown value. It is different from `0`, an empty string, or `FALSE`.

### Incorrect

```sql
SELECT *
FROM employees
WHERE salary = NULL;
```

### Correct

```sql
SELECT *
FROM employees
WHERE salary IS NULL;
```

To find rows where the salary is known:

```sql
SELECT *
FROM employees
WHERE salary IS NOT NULL;
```

**Interview rule:** Use `IS NULL` and `IS NOT NULL`; do not use `= NULL` to test for missing values.

## 14. LIKE — Pattern Matching

`LIKE` searches for text matching a specified pattern.

| Wildcard | Meaning |
|---|---|
| `%` | Zero or more characters |
| `_` | Exactly one character |

### Examples

Names starting with `A`:

```sql
SELECT *
FROM employees
WHERE name LIKE 'A%';
```

Names ending with `a`:

```sql
SELECT *
FROM employees
WHERE name LIKE '%a';
```

Names containing `an`:

```sql
SELECT *
FROM employees
WHERE name LIKE '%an%';
```

Names containing exactly four characters:

```sql
SELECT *
FROM employees
WHERE name LIKE '____';
```

### PostgreSQL: LIKE vs ILIKE

- `LIKE`: Case-sensitive pattern matching in PostgreSQL.
- `ILIKE`: Case-insensitive pattern matching in PostgreSQL.

```sql
SELECT *
FROM employees
WHERE name ILIKE 'a%';
```

This matches names beginning with either uppercase or lowercase `a`.

## 15. COALESCE — Handle NULL Values

`COALESCE` returns the first non-`NULL` value in its arguments.

```sql
SELECT name, COALESCE(salary, 0) AS salary
FROM employees;
```

If `salary` is `NULL`, the result displays `0`.

**Important:** `COALESCE` changes the query result, not the stored database value.

## 16. Combining SQL Clauses

```sql
SELECT name, salary
FROM employees
WHERE salary >= 50000
ORDER BY salary DESC
LIMIT 3;
```

This retrieves the names and salaries of employees earning at least `50000`, sorts them from highest to lowest salary, and returns at most three rows.

### Simplified logical processing order

```text
FROM
  ↓
WHERE
  ↓
SELECT
  ↓
ORDER BY
  ↓
LIMIT
```

This is a simplified logical order, not necessarily the physical execution order selected by the database engine.

---

## Quick Interview Revision

- **SQL vs PostgreSQL:** SQL is a query language; PostgreSQL is a database management system.
- **CRUD:** Create, Read, Update, Delete.
- **DDL vs DML:** DDL defines database structures; DML manipulates records.
- **`WHERE`:** Filters rows.
- **`ORDER BY`:** Sorts rows.
- **`LIMIT`:** Restricts the number of returned rows.
- **`DISTINCT`:** Removes duplicate results.
- **`BETWEEN`:** Includes both endpoints.
- **`NULL`:** Test using `IS NULL` or `IS NOT NULL`.
- **`LIKE`:** Pattern matching using `%` and `_`.
- **`ILIKE`:** Case-insensitive pattern matching in PostgreSQL.
- **`COALESCE`:** Returns the first non-`NULL` argument.
- **Missing `WHERE` in `UPDATE` or `DELETE`:** Can affect every row.
- **Missing `ORDER BY`:** Returned row order is not guaranteed.