# Bulk Processing (FORALL, BULK COLLECT)

Bulk processing features like FORALL and BULK COLLECT help improve performance when working with large volumes of data. This section demonstrates how to use these features.

## BULK COLLECT

BULK COLLECT allows you to fetch multiple rows from a query into a collection with a single context switch, reducing overhead.

```sql
DECLARE
    TYPE t_emp IS TABLE OF employees%ROWTYPE;
    l_emps t_emp;
BEGIN
    SELECT * BULK COLLECT INTO l_emps FROM employees WHERE department_id = 10;
    -- Process l_emps collection
END;
```

## FORALL

FORALL enables you to perform DML operations on collections efficiently, minimizing context switches between PL/SQL and SQL engines.

```sql
DECLARE
    TYPE t_ids IS TABLE OF employees.employee_id%TYPE;
    l_ids t_ids := t_ids(101, 102, 103);
BEGIN
    FORALL i IN l_ids.FIRST .. l_ids.LAST
        UPDATE employees SET salary = salary * 1.1 WHERE employee_id = l_ids(i);
END;
```

## Performance Tips

- Use BULK COLLECT to fetch large result sets into PL/SQL collections.
- Use FORALL for mass DML operations to avoid row-by-row processing.
- Limit the number of rows processed at once to avoid memory issues (use `LIMIT` clause with BULK COLLECT).

## Example: Bulk Insert

```sql
DECLARE
    TYPE t_names IS TABLE OF employees.last_name%TYPE;
    l_names t_names := t_names('Smith', 'Johnson', 'Williams');
BEGIN
    FORALL i IN l_names.FIRST .. l_names.LAST
        INSERT INTO employees (last_name) VALUES (l_names(i));
END;
```


## Comparision of Single Record vs Multi Record (Bulk) Fetch

```sql
DECLARE
  emp_record employees%rowtype; -- Regular Variable (Single Record only)
  TYPE emp_collection is TABLE OF employees%ROWTYPE; -- Collection type
  emp_list emp_collection;
BEGIN
    -- Fetch one record in emp_record
    SELECT * into emp_record from employees where employee_id = 102;
    -- Fetch multiple records in emp_list
    SELECT * bulk collect into emp_list from employees where department_id = 50;
END;
```

### Explanation

This example demonstrates the difference between fetching a single record and multiple records in PL/SQL:

- **Single Record Fetch:**  
  `emp_record employees%rowtype;` declares a variable to hold one row from the `employees` table.  
  `SELECT * INTO emp_record FROM employees WHERE employee_id = 102;` fetches a single employee whose ID is 102.

- **Bulk (Multi-Record) Fetch:**  
  `TYPE emp_collection IS TABLE OF employees%ROWTYPE;` defines a collection type for multiple rows.  
  `emp_list emp_collection;` declares a variable of that collection type.  
  `SELECT * BULK COLLECT INTO emp_list FROM employees WHERE department_id = 50;` fetches all employees in department 50 into the collection.

**Key Point:**  
Single-record fetch uses a regular variable and `SELECT ... INTO`.  
Bulk fetch uses a collection and `SELECT ... BULK COLLECT INTO` for efficiency when handling multiple rows.
