# Example  Scanario

## Excercise

### Create Tables

- products with columns: 
    * product_id (PK)
    * p_name
    * p_description.

- sales_orders with columns: 
    
    * order_id (PK)
    * order_date
    * customer_name
    * order_details
    * total_amount, total_quantity. 

- order_details with columns: 
    * order_id (FK to sales_orders)
    * product_id (FK to products)
    * quantity
    * rate.


###  Sample Data

- Insert at least 5 products in products table.
- Insert 10 orders into the sales_orders table.
- Each order should have between 2 and 5 products in the order_details table.

> Keep the columns `total_price` and `total_quantity` to `NULL`

### Stored Procedures 

- Write a stored procedure to calculate and update the total_amount and total_quantity for each order in the sales_orders table.

## Solution

1. Create tables

```sql
CREATE TABLE products (
    product_id NUMBER PRIMARY KEY,
    p_name VARCHAR2(100),
    p_description VARCHAR2(255)
);

CREATE TABLE sales_orders (
    order_id NUMBER PRIMARY KEY,
    order_date DATE,
    customer_name VARCHAR2(100),
    total_amount NUMBER,
    total_quantity NUMBER
);

CREATE TABLE order_details (
    order_id NUMBER REFERENCES sales_orders(order_id),
    product_id NUMBER REFERENCES products(product_id),
    quantity NUMBER,
    rate NUMBER
);
```

2. Insert products

```sql
INSERT INTO products VALUES (1, 'Laptop', 'High performance laptop');
INSERT INTO products VALUES (2, 'Mouse', 'Wireless mouse');
INSERT INTO products VALUES (3, 'Keyboard', 'Mechanical keyboard');
INSERT INTO products VALUES (4, 'Monitor', '24-inch monitor');
INSERT INTO products VALUES (5, 'Printer', 'Laser printer');
```

3. Insert orders and order details (example for 2 orders)

```sql
INSERT INTO sales_orders VALUES (101, SYSDATE, 'Alice', NULL, NULL);
INSERT INTO sales_orders VALUES (102, SYSDATE, 'Bob', NULL, NULL);

INSERT INTO order_details VALUES (101, 1, 2, 50000);
INSERT INTO order_details VALUES (101, 2, 1, 1500);

INSERT INTO order_details VALUES (102, 3, 1, 3000);
INSERT INTO order_details VALUES (102, 4, 2, 12000);
INSERT INTO order_details VALUES (102, 5, 1, 8000);
```

4. Stored procedure to calculate totals

```sql
CREATE OR REPLACE PROCEDURE update_order_totals IS
BEGIN
    FOR rec IN (SELECT order_id FROM sales_orders) LOOP
        UPDATE sales_orders
        SET
            total_amount = (SELECT SUM(quantity * rate) FROM order_details WHERE order_id = rec.order_id),
            total_quantity = (SELECT SUM(quantity) FROM order_details WHERE order_id = rec.order_id)
        WHERE order_id = rec.order_id;
    END LOOP;
END;
/
```

5. Execute the procedure

```sql
BEGIN
    update_order_totals;
END;
/
```

6. Test the data in `order` table

```sql
select * from orders;
```

