# Triggers in PL/SQL

Triggers are special PL/SQL blocks that execute automatically in response to specific events on a table, view, schema, or database. They are commonly used for auditing, enforcing business rules, and maintaining data integrity.

## Types of Triggers

1. **Row-level Triggers**: Fire once for each row affected by the triggering event.
2. **Statement-level Triggers**: Fire once for the triggering event, regardless of how many rows are affected.
3. **BEFORE Triggers**: Execute before the triggering DML event (INSERT, UPDATE, DELETE).
4. **AFTER Triggers**: Execute after the triggering DML event.
5. **INSTEAD OF Triggers**: Used on views to perform DML operations.

## Syntax of a Trigger

```sql
CREATE [OR REPLACE] TRIGGER trigger_name
{BEFORE | AFTER | INSTEAD OF} [INSERT | UPDATE | DELETE]
ON table_name
[FOR EACH ROW]
DECLARE
	-- variable declarations
BEGIN
	-- trigger logic
END;
/ 
```

## Example 1: Row-level BEFORE INSERT Trigger

Suppose you want to automatically set the `created_at` column to the current date whenever a new row is inserted into the `employees` table:

```sql
CREATE OR REPLACE TRIGGER trg_set_created_at
BEFORE INSERT ON employees
FOR EACH ROW
BEGIN
	:NEW.created_at := SYSDATE;
END;
/ 
```

## Example 2: AFTER UPDATE Statement-level Trigger

Suppose you want to log updates to the `departments` table:

```sql
CREATE OR REPLACE TRIGGER trg_log_department_update
AFTER UPDATE ON departments
BEGIN
	INSERT INTO audit_log (table_name, action, action_date)
	VALUES ('DEPARTMENTS', 'UPDATE', SYSDATE);
END;
/ 
```

## Example 3: INSTEAD OF Trigger on a View

Suppose you have a view that joins multiple tables and you want to allow inserts through the view:

```sql
CREATE OR REPLACE VIEW emp_dept_view AS
SELECT e.emp_id, e.emp_name, d.dept_name
FROM employees e JOIN departments d ON e.dept_id = d.dept_id;

CREATE OR REPLACE TRIGGER trg_instead_of_insert
INSTEAD OF INSERT ON emp_dept_view
BEGIN
	INSERT INTO employees (emp_id, emp_name, dept_id)
	VALUES (:NEW.emp_id, :NEW.emp_name, (SELECT dept_id FROM departments WHERE dept_name = :NEW.dept_name));
END;
/ 
```

## Accessing OLD and NEW Values

In row-level triggers, you can reference the old and new values using `:OLD` and `:NEW` qualifiers:

```sql
CREATE OR REPLACE TRIGGER trg_salary_audit
AFTER UPDATE OF salary ON employees
FOR EACH ROW
BEGIN
	INSERT INTO salary_audit (emp_id, old_salary, new_salary, change_date)
	VALUES (:OLD.emp_id, :OLD.salary, :NEW.salary, SYSDATE);
END;
/ 
```

## Disabling and Dropping Triggers

```sql
ALTER TRIGGER trg_set_created_at DISABLE;
DROP TRIGGER trg_set_created_at;
```

## Best Practices

- Keep trigger logic simple and efficient.
- Avoid using triggers for complex business logic.
- Document triggers clearly.
- Be cautious of mutating table errors (when a trigger tries to modify the table it is triggered on).

---
This section introduced PL/SQL triggers, their types, syntax, and practical examples. Practice creating and testing triggers to reinforce your understanding.
