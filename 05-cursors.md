# Cursors

Cursors are used to process individual rows returned by database queries. This section explains explicit and implicit cursors, cursor attributes, and cursor operations.

## 1. Implicit Cursors

Implicit cursors are automatically created by Oracle when a SQL statement (such as `SELECT INTO`, `INSERT`, `UPDATE`, or `DELETE`) is executed. You do not need to declare them explicitly.

### Example: Using Implicit Cursor with HR Schema

Suppose you want to get the salary of an employee from the HR schema:

```plsql
DECLARE
	v_salary employees.salary%TYPE;
BEGIN
	SELECT salary INTO v_salary
	FROM employees
	WHERE employee_id = 100;
	DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
END;
/ 
```

#### Implicit Cursor Attributes
After executing DML statements, you can use implicit cursor attributes:
- `SQL%ROWCOUNT` – Number of rows affected
- `SQL%FOUND` – Returns TRUE if at least one row was affected
- `SQL%NOTFOUND` – Returns TRUE if no rows were affected
- `SQL%ISOPEN` – Always FALSE for implicit cursors

Example:
```plsql
UPDATE employees SET salary = salary * 1.1 WHERE department_id = 10;
DBMS_OUTPUT.PUT_LINE('Rows updated: ' || SQL%ROWCOUNT);
```

---

## 2. Explicit Cursors

Explicit cursors are declared by the programmer for queries that return more than one row. You control the cursor's open, fetch, and close operations.

### Steps to Use Explicit Cursors
1. **Declare** the cursor
2. **Open** the cursor
3. **Fetch** rows from the cursor
4. **Close** the cursor

### Example: Explicit Cursor for HR Schema

Suppose you want to list all employees in department 10:

```plsql
DECLARE
	CURSOR emp_cur IS
		SELECT employee_id, first_name, last_name, salary
		FROM employees
		WHERE department_id = 10;
	v_emp_id   employees.employee_id%TYPE;
	v_fname    employees.first_name%TYPE;
	v_lname    employees.last_name%TYPE;
	v_salary   employees.salary%TYPE;
BEGIN
	OPEN emp_cur;
	LOOP
		FETCH emp_cur INTO v_emp_id, v_fname, v_lname, v_salary;
		EXIT WHEN emp_cur%NOTFOUND;
		DBMS_OUTPUT.PUT_LINE(v_emp_id || ': ' || v_fname || ' ' || v_lname || ', Salary: ' || v_salary);
	END LOOP;
	CLOSE emp_cur;
END;
/ 
```

### Example: FOR Loop with Cursor

PL/SQL provides a shortcut for processing explicit cursors using a FOR loop. The FOR loop automatically opens, fetches, and closes the cursor.

Suppose you want to print all employees in department 20:

```plsql
FOR rec IN (
	SELECT employee_id, first_name, last_name, salary
	FROM employees
	WHERE department_id = 20
) LOOP
	DBMS_OUTPUT.PUT_LINE(rec.employee_id || ': ' || rec.first_name || ' ' || rec.last_name || ', Salary: ' || rec.salary);
END LOOP;
```

Or, using a named cursor:

```plsql
DECLARE
	CURSOR emp_cur IS
		SELECT employee_id, first_name, last_name, salary
		FROM employees
		WHERE department_id = 30;
BEGIN
	FOR rec IN emp_cur LOOP
		DBMS_OUTPUT.PUT_LINE(rec.employee_id || ': ' || rec.first_name || ' ' || rec.last_name || ', Salary: ' || rec.salary);
	END LOOP;
END;
/ 
```

The FOR loop simplifies cursor handling and is recommended for most use cases where you need to process each row of a query.

#### Explicit Cursor Attributes
- `emp_cur%ROWCOUNT` – Number of rows fetched so far
- `emp_cur%FOUND` – TRUE if last fetch returned a row
- `emp_cur%NOTFOUND` – TRUE if last fetch did not return a row
- `emp_cur%ISOPEN` – TRUE if cursor is open

---

## 3. Summary Table

| Feature                | Implicit Cursor         | Explicit Cursor         |
|------------------------|------------------------|------------------------|
| Declaration            | Automatic              | Programmer-defined     |
| Control                | Automatic              | Manual (open/fetch/close) |
| For Multi-row Queries  | No                     | Yes                    |
| Cursor Attributes      | SQL%                   | cursor_name%           |

---

## 4. Best Practices
- Use implicit cursors for single-row queries or DML statements.
- Use explicit cursors for multi-row queries that require row-by-row processing.

---

**References:**
- Oracle 19c Documentation: [PL/SQL Cursors](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/plsql-cursors.html)
