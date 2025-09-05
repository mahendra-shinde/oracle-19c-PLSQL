# Records and Collections

PL/SQL provides composite data types such as records and collections (associative arrays, nested tables, and VARRAYs). This section explains how to use these data types effectively.
## Records

A record is a composite data structure that groups related data items of different types. You can define your own record types using the `TYPE` statement.

**Example: Declaring and Using a Record**

```plsql
DECLARE
    TYPE emp_record IS RECORD (
        employee_id NUMBER,
        first_name  VARCHAR2(50),
        salary      NUMBER
    );
    emp_rec emp_record;
BEGIN
    emp_rec.employee_id := 101;
    emp_rec.first_name := 'John';
    emp_rec.salary := 5000;
    DBMS_OUTPUT.PUT_LINE('ID: ' || emp_rec.employee_id || ', Name: ' || emp_rec.first_name || ', Salary: ' || emp_rec.salary);
END;
```

## Collections

Collections are ordered groups of elements, all of the same type. PL/SQL supports three types of collections:

- **Associative Arrays (Index-by Tables)**
- **Nested Tables**
- **VARRAYs (Variable-size Arrays)**

### Associative Arrays

Associative arrays use arbitrary numbers or strings as indexes.

**Example: Associative Array for Employee Salaries**

```plsql
DECLARE
    TYPE salary_table IS TABLE OF NUMBER INDEX BY VARCHAR2(50);
    salaries salary_table;
BEGIN
    salaries('KING') := 24000;
    salaries('CLARK') := 10000;
    DBMS_OUTPUT.PUT_LINE('KING''s Salary: ' || salaries('KING'));
END;
```

### Nested Tables

Nested tables can be stored in database columns and manipulated like arrays.

**Example: Nested Table of Department Names**

```plsql
DECLARE
    TYPE dept_table IS TABLE OF VARCHAR2(50);
    departments dept_table := dept_table('HR', 'SALES', 'IT');
BEGIN
    FOR i IN departments.FIRST .. departments.LAST LOOP
        DBMS_OUTPUT.PUT_LINE('Department: ' || departments(i));
    END LOOP;
END;
```

### VARRAYs

VARRAYs have a fixed maximum size.

**Example: VARRAY of Job Titles**

```plsql
DECLARE
    TYPE job_varray IS VARRAY(3) OF VARCHAR2(30);
    jobs job_varray := job_varray('MANAGER', 'CLERK', 'ANALYST');
BEGIN
    FOR i IN jobs.FIRST .. jobs.LAST LOOP
        DBMS_OUTPUT.PUT_LINE('Job Title: ' || jobs(i));
    END LOOP;
END;
```

## HR Schema Examples

**Fetch Employee Data into a Record**

```plsql
DECLARE
    emp_rec employees%ROWTYPE;
BEGIN
    SELECT * INTO emp_rec FROM employees WHERE employee_id = 100;
    DBMS_OUTPUT.PUT_LINE('Name: ' || emp_rec.first_name || ' ' || emp_rec.last_name);
END;
```

**Store Multiple Employee IDs in a Collection**

```plsql
DECLARE
    TYPE emp_id_table IS TABLE OF NUMBER;
    emp_ids emp_id_table;
BEGIN
    SELECT employee_id BULK COLLECT INTO emp_ids FROM employees WHERE department_id = 10;
    FOR i IN emp_ids.FIRST .. emp_ids.LAST LOOP
        DBMS_OUTPUT.PUT_LINE('Employee ID: ' || emp_ids(i));
    END LOOP;
END;
```

Use records and collections to organize and process related data efficiently in PL/SQL programs.