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