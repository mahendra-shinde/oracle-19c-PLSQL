
# Control Structures (IF, CASE, LOOP)

## Logical and Comparison Operators in PL/SQL

Logical and comparison operators are used to form conditions in control statements.

### Comparison Operators
| Operator | Description           | Example           |
|----------|-----------------------|-------------------|
| =        | Equal to              | a = b             |
| !=, <>   | Not equal to          | a != b, a <> b    |
| >        | Greater than          | a > b             |
| <        | Less than             | a < b             |
| >=       | Greater than or equal | a >= b            |
| <=       | Less than or equal    | a <= b            |

**Example:**
```plsql
IF salary >= 5000 THEN
	-- statements
END IF;
```

### Logical Operators
| Operator | Description           | Example                |
|----------|-----------------------|------------------------|
| AND      | Logical AND           | a > 0 AND b < 10       |
| OR       | Logical OR            | a = 1 OR b = 2         |
| NOT      | Logical NOT           | NOT (a = b)            |

**Example:**
```plsql
IF (age > 18 AND status = 'ACTIVE') THEN
	-- statements
END IF;
```

Control structures allow you to control the flow of execution in your PL/SQL programs. This section covers IF statements, CASE statements, and various types of loops.

## 1. IF Statements

The IF statement is used to execute a block of code if a specified condition is true.

**Syntax:**
```plsql
IF condition THEN
	 -- statements
END IF;
```

**Example:**
```plsql
DECLARE
	v_num NUMBER := 10;
BEGIN
	IF v_num > 5 THEN
		DBMS_OUTPUT.PUT_LINE('v_num is greater than 5');
	END IF;
END;
/ 
```

## 2. IF-ELSE and IF-ELSIF-ELSE

You can extend the IF statement with ELSE and ELSIF clauses.

**Syntax:**
```plsql
IF condition1 THEN
	 -- statements
ELSIF condition2 THEN
	 -- statements
ELSE
	 -- statements
END IF;
```

**Example:**
```plsql
DECLARE
	v_score NUMBER := 75;
BEGIN
	IF v_score >= 90 THEN
		DBMS_OUTPUT.PUT_LINE('Grade: A');
	ELSIF v_score >= 75 THEN
		DBMS_OUTPUT.PUT_LINE('Grade: B');
	ELSE
		DBMS_OUTPUT.PUT_LINE('Grade: C');
	END IF;
END;
/ 
```

## 3. CASE Statements

The CASE statement is used for conditional logic based on multiple possible values.

**Syntax:**
```plsql
CASE expression
	WHEN value1 THEN
		-- statements
	WHEN value2 THEN
		-- statements
	ELSE
		-- statements
END CASE;
```

**Example:**
```plsql
DECLARE
	v_day VARCHAR2(10) := 'MONDAY';
BEGIN
	CASE v_day
		WHEN 'MONDAY' THEN
			DBMS_OUTPUT.PUT_LINE('Start of the week');
		WHEN 'FRIDAY' THEN
			DBMS_OUTPUT.PUT_LINE('End of the work week');
		ELSE
			DBMS_OUTPUT.PUT_LINE('Midweek');
	END CASE;
END;
/ 
```

## 4. Loops

PL/SQL supports several types of loops: simple LOOP, WHILE LOOP, and FOR LOOP.

### a) Simple LOOP
Repeats a block of code until an EXIT statement is encountered.

**Example:**
```plsql
DECLARE
	v_counter NUMBER := 1;
BEGIN
	LOOP
		DBMS_OUTPUT.PUT_LINE('Counter: ' || v_counter);
		v_counter := v_counter + 1;
		EXIT WHEN v_counter > 5;
	END LOOP;
END;
/ 
```

### b) WHILE LOOP
Repeats a block of code while a condition is true.

**Example:**
```plsql
DECLARE
	v_counter NUMBER := 1;
BEGIN
	WHILE v_counter <= 5 LOOP
		DBMS_OUTPUT.PUT_LINE('Counter: ' || v_counter);
		v_counter := v_counter + 1;
	END LOOP;
END;
/ 
```

### c) FOR LOOP
Iterates over a range of values.

**Example:**
```plsql
BEGIN
	FOR v_counter IN 1..5 LOOP
		DBMS_OUTPUT.PUT_LINE('Counter: ' || v_counter);
	END LOOP;
END;
/ 
```

---
These control structures are fundamental for writing effective PL/SQL programs. Practice using them to build logic in your code.
