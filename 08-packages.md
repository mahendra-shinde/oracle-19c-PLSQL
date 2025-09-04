# Packages in PL/SQL

Packages are collections of related procedures, functions, variables, cursors, and other PL/SQL constructs grouped together as a single unit. They help organize code, promote reusability, and improve performance by loading all package elements into memory at once.

## Benefits of Packages

- **Modularity:** Group related procedures and functions together.
- **Encapsulation:** Hide implementation details and expose only necessary components.
- **Easier Maintenance:** Update package body without affecting the specification.
- **Performance:** Loaded into memory once, reducing disk I/O.

## Structure of a Package

A package consists of two parts:

1. **Package Specification:** Declares public procedures, functions, variables, types, etc.
2. **Package Body:** Implements the procedures and functions declared in the specification.

### Example: Simple Package

#### 1. Package Specification
```sql
CREATE OR REPLACE PACKAGE emp_pkg AS
	PROCEDURE add_employee(p_id NUMBER, p_name VARCHAR2);
	FUNCTION get_employee(p_id NUMBER) RETURN VARCHAR2;
END emp_pkg;
/ 
```

#### 2. Package Body
```sql
CREATE OR REPLACE PACKAGE BODY emp_pkg AS
	-- Simple in-memory table simulation
	TYPE emp_table_type IS TABLE OF VARCHAR2(100) INDEX BY BINARY_INTEGER;
	emp_table emp_table_type;

	PROCEDURE add_employee(p_id NUMBER, p_name VARCHAR2) IS
	BEGIN
		emp_table(p_id) := p_name;
	END;

	FUNCTION get_employee(p_id NUMBER) RETURN VARCHAR2 IS
	BEGIN
		RETURN emp_table(p_id);
	END;
END emp_pkg;
/ 
```

## Using the Package

```sql
BEGIN
	emp_pkg.add_employee(1, 'John Doe');
	DBMS_OUTPUT.PUT_LINE(emp_pkg.get_employee(1)); -- Output: John Doe
END;
/ 
```

## More Features

- **Private Elements:** Declarations in the body but not in the spec are private.
- **Initialization Section:** Code at the end of the body runs once when the package is first referenced.

### Example: Private Procedure and Initialization
```sql
CREATE OR REPLACE PACKAGE demo_pkg AS
	PROCEDURE show_message;
END demo_pkg;
/ 

CREATE OR REPLACE PACKAGE BODY demo_pkg AS
	PROCEDURE private_proc IS
	BEGIN
		DBMS_OUTPUT.PUT_LINE('This is private');
	END;

	PROCEDURE show_message IS
	BEGIN
		DBMS_OUTPUT.PUT_LINE('Hello from package!');
		private_proc;
	END;

BEGIN
	DBMS_OUTPUT.PUT_LINE('Package demo_pkg initialized!');
END demo_pkg;
/ 
```

## Summary

- Packages help organize and encapsulate PL/SQL code.
- Use the specification for public declarations and the body for implementations and private code.
- Initialization code runs once per session.
