# Dynamic SQL

Dynamic SQL allows you to construct and execute SQL statements dynamically at runtime. This section covers the EXECUTE IMMEDIATE statement and DBMS_SQL package.
## EXECUTE IMMEDIATE

The `EXECUTE IMMEDIATE` statement is used to execute a single SQL statement or PL/SQL block dynamically. It is simple and efficient for most dynamic SQL needs.

**Example:**
```plsql
DECLARE
    sql_stmt VARCHAR2(100);
BEGIN
    sql_stmt := 'UPDATE employees SET salary = salary * 1.1 WHERE department_id = 10';
    EXECUTE IMMEDIATE sql_stmt;
END;
```

### Using Bind Variables

Bind variables can be used with `EXECUTE IMMEDIATE` to pass values safely.

```plsql
DECLARE
    sql_stmt VARCHAR2(100);
    dept_id NUMBER := 20;
BEGIN
    sql_stmt := 'DELETE FROM employees WHERE department_id = :dept';
    EXECUTE IMMEDIATE sql_stmt USING dept_id;
END;
```

## DBMS_SQL Package

The `DBMS_SQL` package provides a more flexible interface for dynamic SQL, especially useful for statements with unknown numbers of columns or parameters.

**Basic Steps:**
1. Open a cursor.
2. Parse the SQL statement.
3. Bind variables (if needed).
4. Define columns (for queries).
5. Execute the statement.
6. Fetch results (for queries).
7. Close the cursor.

**Example:**
```plsql
DECLARE
    cur INTEGER;
    rows_processed INTEGER;
BEGIN
    cur := DBMS_SQL.OPEN_CURSOR;
    DBMS_SQL.PARSE(cur, 'UPDATE employees SET salary = salary * 1.05', DBMS_SQL.NATIVE);
    rows_processed := DBMS_SQL.EXECUTE(cur);
    DBMS_SQL.CLOSE_CURSOR(cur);
END;
```

## When to Use Each

- Use `EXECUTE IMMEDIATE` for simple, single-statement dynamic SQL.
- Use `DBMS_SQL` for complex scenarios, such as dynamic queries with unknown columns or bulk operations.

## Best Practices

- Always validate and sanitize dynamic SQL to prevent SQL injection.
- Prefer bind variables to improve performance and security.
- Close cursors to free resources.
