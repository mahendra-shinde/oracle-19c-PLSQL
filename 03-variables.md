# Variables and Data Types in PL/SQL

Variables are fundamental to PL/SQL programming. They allow you to store, manipulate, and retrieve data during the execution of a PL/SQL block. Understanding how to declare, use, and manage variables is essential for writing effective PL/SQL code.

## 1. Declaring Variables

Variables are declared in the **declaration section** of a PL/SQL block using the following syntax:

```plsql
variable_name [CONSTANT] datatype [NOT NULL] [:= initial_value];
```

**Example:**
```plsql
DECLARE
  l_employee_id   NUMBER;
  l_employee_name VARCHAR2(50) := 'John Doe';
  l_hire_date     DATE := SYSDATE;
  l_is_active     BOOLEAN := TRUE;
BEGIN
  -- Use variables here
END;
/ 
```

## 2. Common Data Types

PL/SQL supports a variety of data types:

- **NUMBER(p,s):** Stores numeric data. `p` is precision, `s` is scale.
- **VARCHAR2(n):** Variable-length character string.
- **CHAR(n):** Fixed-length character string.
- **DATE:** Stores date and time information.
- **BOOLEAN:** Stores TRUE, FALSE, or NULL.
- **BINARY_INTEGER, PLS_INTEGER:** For integer values.

**Example:**
```plsql
l_salary NUMBER(8,2) := 50000.00;
l_department VARCHAR2(30);
```

## 3. Variable Initialization and Assignment

Variables can be initialized at the time of declaration or assigned values later using the `:=` operator.

```plsql
l_counter NUMBER := 0;
l_counter := l_counter + 1;
```

## 4. Scope and Lifetime

- **Local variables** are accessible only within the block where they are declared.
- **Global variables** can be declared in packages for use across multiple blocks.

**Example:**
```plsql
DECLARE
  l_message VARCHAR2(100);
BEGIN
  l_message := 'Hello, PL/SQL!';
  DBMS_OUTPUT.PUT_LINE(l_message);
END;
/ 
```

## 5. Naming Conventions and Best Practices

- Use meaningful names (e.g., `l_total_salary` instead of `x`).
- Prefix local variables with `l_`, parameters with `p_`, and global/package variables with `g_`.
- Avoid using reserved words as variable names.
- Use UPPERCASE for constants.

## 6. Constants

Declare constants using the `CONSTANT` keyword. Their value cannot be changed after initialization.

```plsql
pi CONSTANT NUMBER := 3.14159;
```

## 7. Anchored Declarations

Use `%TYPE` and `%ROWTYPE` to anchor variable types to table columns or rows.

```plsql
l_emp_name employees.last_name%TYPE;
l_emp_record employees%ROWTYPE;
```

## 8. Example: Using Variables in a PL/SQL Block

```plsql
DECLARE
  l_id         NUMBER := 100;
  l_name       VARCHAR2(50);
  l_hire_date  DATE;
BEGIN
  l_name := 'Alice';
  l_hire_date := SYSDATE;
  DBMS_OUTPUT.PUT_LINE('ID: ' || l_id || ', Name: ' || l_name || ', Hire Date: ' || l_hire_date);
END;
/ 
```

---

**Summary:**

- Declare variables in the declaration section.
- Choose appropriate data types.
- Use meaningful names and follow best practices.
- Understand variable scope and lifetime.

Practice declaring and using variables to build a strong foundation in PL/SQL programming.
