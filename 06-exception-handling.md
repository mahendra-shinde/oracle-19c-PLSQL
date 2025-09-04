# Exception Handling in PL/SQL 

PL/SQL provides a robust mechanism for handling errors and exceptions that may occur during program execution. Exception handling allows you to **gracefully manage errors** instead of letting your program terminate abruptly.


## Structure of a PL/SQL Block with Exception Handling

A PL/SQL block generally has **three sections**:

```plsql
DECLARE
   -- Variable and cursor declarations (optional)
BEGIN
   -- Executable statements
EXCEPTION
   -- Exception handlers
END;
/
```

---

## 🔹 Types of Exceptions in PL/SQL

1. **Predefined Exceptions**

   * Automatically defined by Oracle.
   * Examples:

     * `ZERO_DIVIDE` → Division by zero
     * `NO_DATA_FOUND` → SELECT INTO returns no rows
     * `TOO_MANY_ROWS` → SELECT INTO returns more than one row

2. **Non-predefined Exceptions**

   * Raised by Oracle but not predefined.
   * Must be declared in the `DECLARE` section and linked using `PRAGMA EXCEPTION_INIT`.

3. **User-defined Exceptions**

   * Defined and raised explicitly by the programmer using `RAISE`.

## Handling Predefined Exceptions

Example: Handling **divide by zero** error.

```plsql
DECLARE
    v_num     NUMBER := 10;
    v_den     NUMBER := 0;
    v_result  NUMBER;
BEGIN
    v_result := v_num / v_den;
    DBMS_OUTPUT.PUT_LINE('Result: ' || v_result);

EXCEPTION
    WHEN ZERO_DIVIDE THEN
        DBMS_OUTPUT.PUT_LINE('Error: Division by zero is not allowed.');
END;
/
```

## Handling Multiple Exceptions

You can handle more than one exception in a block:

```plsql
DECLARE
    v_name  VARCHAR2(50);
BEGIN
    -- This will raise NO_DATA_FOUND if no row is returned
    SELECT first_name INTO v_name FROM employees WHERE employee_id = -1;

    DBMS_OUTPUT.PUT_LINE('Employee: ' || v_name);

EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: No employee found with that ID.');
    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('Error: More than one employee found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected Error: ' || SQLERRM);
END;
/
```

## User-defined Exceptions

You can create and raise your own exceptions:

```plsql
DECLARE
    e_invalid_salary EXCEPTION;  -- Declare custom exception
    v_salary NUMBER := -5000;
BEGIN
    IF v_salary < 0 THEN
        RAISE e_invalid_salary;  -- Raise custom exception
    END IF;

    DBMS_OUTPUT.PUT_LINE('Salary is valid: ' || v_salary);

EXCEPTION
    WHEN e_invalid_salary THEN
        DBMS_OUTPUT.PUT_LINE('Error: Salary cannot be negative.');
END;
/
```

##  Using `PRAGMA EXCEPTION_INIT`

This allows you to associate a **non-predefined exception** with an Oracle error number.

```plsql
DECLARE
    e_deadlock_detected EXCEPTION;
    PRAGMA EXCEPTION_INIT(e_deadlock_detected, -60); -- ORA-00060
BEGIN
    -- Some code that may cause a deadlock
    NULL;

EXCEPTION
    WHEN e_deadlock_detected THEN
        DBMS_OUTPUT.PUT_LINE('Error: Deadlock detected.');
END;
/
```

## Best Practices

* Always handle **common exceptions** like `NO_DATA_FOUND`, `ZERO_DIVIDE`, and `TOO_MANY_ROWS`.
* Use `WHEN OTHERS` as the **last exception handler**, not the first.
* Log unexpected errors using `SQLERRM` for debugging.
* Avoid suppressing exceptions without proper handling (don’t leave empty handlers).
