
# PL/SQL Block Structure

PL/SQL programs are built from logical blocks. Each block is a self-contained unit of code that can include declarations, executable statements, and exception handlers. Understanding the block structure is fundamental to writing effective PL/SQL code.

## Sections of a PL/SQL Block

1. **Declaration Section (optional):**
	- Begins with the `DECLARE` keyword.
	- Used to define variables, constants, cursors, and user-defined types.
	- This section is optional; if not needed, you can omit it.

2. **Execution Section (mandatory):**
	- Begins with the `BEGIN` keyword.
	- Contains the statements that are executed, such as assignments, SQL queries, and control structures.
	- This section is required in every block.

3. **Exception Handling Section (optional):**
	- Begins with the `EXCEPTION` keyword.
	- Used to handle runtime errors and exceptions that occur during execution.
	- This section is optional.

4. **End of Block:**
	- Every block ends with the `END;` statement.

## Syntax of a PL/SQL Block

```plsql
DECLARE
	-- Declarations (optional)
BEGIN
	-- Executable statements (mandatory)
EXCEPTION
	-- Exception handling (optional)
END;
```

## Types of PL/SQL Blocks

- **Anonymous Blocks:**
  - Not named or stored in the database.
  - Used for ad-hoc operations and scripts.
  - Example:
	 ```plsql
	 BEGIN
		 DBMS_OUTPUT.PUT_LINE('Hello, PL/SQL!');
	 END;
	 ```

- **Named Blocks:**
  - Stored in the database as procedures, functions, packages, or triggers.
  - Can be invoked by name and reused.
  - Example (Procedure):
	 ```plsql
	 CREATE OR REPLACE PROCEDURE greet_user IS
	 BEGIN
		 DBMS_OUTPUT.PUT_LINE('Welcome to PL/SQL!');
	 END;
	 ```

## Nested Blocks

PL/SQL allows blocks to be nested within other blocks. This enables modular programming and better error handling.

Example of a nested block:

```plsql
DECLARE
	outer_var NUMBER := 10;
BEGIN
	DBMS_OUTPUT.PUT_LINE('Outer block: ' || outer_var);
	DECLARE
		inner_var NUMBER := 20;
	BEGIN
		DBMS_OUTPUT.PUT_LINE('Inner block: ' || inner_var);
	END;
END;
```

## Summary

PL/SQL block structure is the foundation of all PL/SQL programs. Mastery of block structure enables you to write clear, maintainable, and robust code.

For more details, refer to the [Oracle Database PL/SQL Language Reference 19c](https://docs.oracle.com/en/database/oracle/oracle-database/19/lnpls/index.html).
