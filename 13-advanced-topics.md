# Advanced Topics

## Table of Contents

- [Performance Tuning](#performance-tuning)
- [Security in PL/SQL](#security-in-plsql)
- [Advanced Error Handling](#advanced-error-handling)
- [Best Practices](#best-practices)

---

## Performance Tuning

Optimizing PL/SQL code is crucial for high-performance applications. Key techniques include:

- **Bulk Processing**: Use `FORALL` and `BULK COLLECT` to process multiple rows with fewer context switches.

**Example:**
```plsql
DECLARE
	TYPE numtab IS TABLE OF NUMBER;
	l_ids numtab;
BEGIN
	SELECT employee_id BULK COLLECT INTO l_ids FROM employees;
	FORALL i IN l_ids.FIRST .. l_ids.LAST
		UPDATE employees SET salary = salary * 1.1 WHERE employee_id = l_ids(i);
END;
```

- **Minimize Context Switches**: Avoid frequent switching between SQL and PL/SQL by grouping operations.
- **Efficient Indexing**: Create indexes on columns used in WHERE clauses to speed up queries.

## Security in PL/SQL

Securing PL/SQL applications protects data and logic. Important concepts:

- **Invoker Rights vs. Definer Rights**: Control who executes code and with what privileges.

**Example:**
```plsql
CREATE OR REPLACE PROCEDURE show_salary AUTHID CURRENT_USER IS
	v_salary NUMBER;
BEGIN
	SELECT salary INTO v_salary FROM employees WHERE employee_id = 101;
	DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
END;
```

- **Privilege Management**: Grant only necessary privileges to users and roles.
- **Data Protection**: Use encryption and auditing for sensitive data.

## Advanced Error Handling

Robust error handling improves reliability and maintainability.

- **Exception Blocks**: Catch and handle errors gracefully.
- **Custom Exceptions**: Define and raise your own exceptions for business logic.
- **Logging**: Record errors for later analysis.

**Example:**
```plsql
DECLARE
	e_salary_error EXCEPTION;
	v_salary NUMBER := -1;
BEGIN
	IF v_salary < 0 THEN
		RAISE e_salary_error;
	END IF;
EXCEPTION
	WHEN e_salary_error THEN
		DBMS_OUTPUT.PUT_LINE('Salary cannot be negative!');
END;
```

## Best Practices

Follow these guidelines for maintainable and efficient PL/SQL code:

- **Modularize Logic**: Break code into procedures and functions.
- **Use Meaningful Names**: Name variables and procedures clearly.
- **Document Code**: Add comments and documentation for clarity.
- **Avoid Hardcoding**: Use constants and parameters.

**Example:**
```plsql
-- Procedure to get employee details
CREATE OR REPLACE PROCEDURE get_employee_details(p_emp_id NUMBER) IS
	v_name employees.first_name%TYPE;
	v_salary employees.salary%TYPE;
BEGIN
	SELECT first_name, salary INTO v_name, v_salary FROM employees WHERE employee_id = p_emp_id;
	DBMS_OUTPUT.PUT_LINE('Name: ' || v_name || ', Salary: ' || v_salary);
END;
```

---
This section covers advanced PL/SQL topics with practical explanations and examples for performance tuning, security, error handling, and best practices.
