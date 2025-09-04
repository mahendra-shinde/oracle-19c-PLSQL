# Stored Procedures and Functions in PL/SQL

Procedures and functions are named PL/SQL blocks that can be stored in the database and reused. They help modularize code, improve reusability, and simplify maintenance.


## Stored Procedures

A **procedure** is a subprogram that performs a specific action. It can accept parameters and can be called from other PL/SQL blocks, triggers, or applications.

### Syntax

```sql
CREATE [OR REPLACE] PROCEDURE procedure_name (
	[parameter1 [mode] datatype, ...]
) IS
BEGIN
	-- procedure body
END procedure_name;
/ 
```

**Parameter Modes:**
- `IN`: (default) Input parameter
- `OUT`: Output parameter
- `IN OUT`: Both input and output

### Example: Simple Procedure

```sql
CREATE OR REPLACE PROCEDURE greet_user(p_name IN VARCHAR2) IS
BEGIN
	DBMS_OUTPUT.PUT_LINE('Hello, ' || p_name || '!');
END greet_user;
/ 
```

**Calling the Procedure:**
```sql
BEGIN
	greet_user('Mahendra');
END;
/ 
```

### Example: Procedure with OUT Parameter

```sql
CREATE OR REPLACE PROCEDURE get_employee_name(
    p_emp_id IN NUMBER,
    p_emp_name OUT VARCHAR2
) IS
BEGIN
    SELECT first_name || ' ' || last_name
    INTO p_emp_name
    FROM employees
    WHERE employee_id = p_emp_id;
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        p_emp_name := 'Not found';
END get_employee_name;
/ 
```

**Using the Procedure in an Anonymous Block:**
```sql
DECLARE
    v_name VARCHAR2(100);
BEGIN
    get_employee_name(101, v_name);
    DBMS_OUTPUT.PUT_LINE('Employee Name: ' || v_name);
END;
/ 
```

## Functions

A **function** is similar to a procedure, but it returns a value and can be used in SQL expressions.

### Syntax

```sql
CREATE [OR REPLACE] FUNCTION function_name (
	[parameter1 [mode] datatype, ...]
) RETURN datatype IS
BEGIN
	-- function body
	RETURN value;
END function_name;
/ 
```

### Example: Simple Function

```sql
CREATE OR REPLACE FUNCTION get_square(p_num IN NUMBER) RETURN NUMBER IS
BEGIN
	RETURN p_num * p_num;
END get_square;
/ 
```

**Calling the Function:**
```sql
DECLARE
	v_result NUMBER;
BEGIN
	v_result := get_square(5);
	DBMS_OUTPUT.PUT_LINE('Square is: ' || v_result);
END;
/ 
```

### Invoking a Function from a SQL Query

You can call a function directly within a SQL statement, such as `SELECT`:

```sql
SELECT get_square(7) AS squared_value FROM dual;
```

---

## Differences Between Procedures and Functions

| Feature         | Procedure         | Function           |
|-----------------|-------------------|--------------------|
| Return Value    | No                | Yes (using RETURN) |
| Use in SQL      | No                | Yes                |
| Call in SELECT  | No                | Yes                |

---

## More Examples

### Procedure with IN and OUT Parameters

```sql
CREATE OR REPLACE PROCEDURE add_numbers(
	p_num1 IN NUMBER,
	p_num2 IN NUMBER,
	p_sum OUT NUMBER
) IS
BEGIN
	p_sum := p_num1 + p_num2;
END add_numbers;
/ 
```

**Calling the Procedure:**
```sql
DECLARE
	v_sum NUMBER;
BEGIN
	add_numbers(10, 20, v_sum);
	DBMS_OUTPUT.PUT_LINE('Sum is: ' || v_sum);
END;
/ 
```

### Function Used in SQL

```sql
CREATE OR REPLACE FUNCTION get_double(p_num IN NUMBER) RETURN NUMBER IS
BEGIN
	RETURN p_num * 2;
END get_double;
/ 
```

**Using in SQL:**
```sql
SELECT get_double(15) AS doubled FROM dual;
```

---

### Example: Function to Return Average Salary

```sql
CREATE OR REPLACE FUNCTION get_avg_salary RETURN NUMBER IS
    v_avg_salary NUMBER;
BEGIN
    SELECT AVG(salary) INTO v_avg_salary FROM employees;
    RETURN v_avg_salary;
END get_avg_salary;
/ 
```

**Calling the Function in a SQL Query:**
```sql
SELECT 
    employee_id,
    salary,
    get_avg_salary() AS average_salary,
    CASE
        WHEN salary > get_avg_salary() THEN 'Above Average'
        WHEN salary < get_avg_salary() THEN 'Below Average'
        ELSE 'Average'
    END AS salary_comparison
FROM employees;
```

## Best Practices
- Use procedures for actions, functions for computations that return a value.
- Always handle exceptions inside procedures/functions.
- Use meaningful names for parameters and subprograms.

---

**Explore more by modifying and running these examples!**
