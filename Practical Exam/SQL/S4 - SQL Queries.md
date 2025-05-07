# S4 - SQL Queries (in MySQL)

**Problem Statement:**
SQL Queries:
Create following tables with suitable constraints (primary key,
foreign key, not null etc).
Insert record and solve the following queries:
Create table Cust_Master(Cust_no, Cust_name, Cust_addr)
Create table Order(Order_no, Cust_no, Order_date, Qty_Ordered)
Create Product (Product_no, Product_name, Order_no)
1. List names of customers having 'A' as second letter in their
name.
2. Display order from Customer no C1002, C1005, C1007 and C1008
3. List Clients who stay in either 'Banglore or 'Manglore'
4. Display name of customers& the product_name they have purchase
5. Create view View1 consisting of Cust_name, Product_name.
6. Disply product_name and quantity purchase by each customer
7. Perform different joint operation.

---

## Creating the database
```sql
CREATE DATABASE Store1;
USE Store1;

```

## Creating tables:

```sql
CREATE TABLE Cust_Master (
  Cust_no VARCHAR(255) NOT NULL,
  Cust_name VARCHAR(255),
  Cust_addr VARCHAR(255),
  PRIMARY KEY (cust_no)
);

CREATE TABLE Orders (
  -- Cannot have 'Order' as table name since it is a keyword reserved for 'ORDER BY' cause
  Order_no INT,
  Cust_no VARCHAR(255),
  Order_date DATE,
  Qty_Ordered INT,
  PRIMARY KEY (Order_no)
);

CREATE TABLE Product (
  Product_no INT,
  Product_name VARCHAR(255),
  Order_no INT
);

```

## Declaring foreign keys

```sql
ALTER TABLE Orders ADD FOREIGN KEY (Cust_no) REFERENCES Cust_Master (Cust_no);
ALTER TABLE Product ADD FOREIGN KEY (Order_no) REFERENCES Orders (Order_no);

```

## Inserting data

```sql
INSERT INTO Cust_Master VALUES
('C1001', 'Kalas', 'Pune'),
('C1002', 'Macho', 'Banglore'),
('C1003', 'Gundeti', 'Chennai'),
('C1005', 'Salvi',  'Manglore'),
('C1006', 'Kshitij', 'Assam'),
('C1007', 'Himashu', 'Banglore'),
('C1008', 'Mehul', 'Mumbai');

INSERT INTO Orders VALUES
(1, 'C1001', '2024-11-09', 10),
(2, 'C1003', '2024-11-01', 5),
(3, 'C1005', '2024-11-05', 45),
(4, 'C1002', '2024-10-29', 3),
(5, 'C1007', '2024-10-15', 2),
(6, 'C1008', '2024-11-10', 7),
(7, 'C1006', '2024-11-09', 1);

INSERT INTO Product VALUES
('101', 'Political Stamps', 1),
('204', 'Fashion Accessory', 2),
('438', 'Complan', 3),
('327', 'ID Card Strap', 4),
('243', 'Face and Hair Wash', 5),
('373', 'Fat Reducer 6000', 6),
('327', 'Personality', 7);

```

## Queries

1. List names of customers having 'A' as second letter in their name.
```sql
SELECT Cust_name FROM Cust_Master WHERE Cust_name LIKE "_a%";

```

2. Display order from Customer no C1002, C1005, C1007 and C1008
```sql
SELECT * FROM Orders WHERE Cust_no IN ('C1002', 'C1005', 'C1007', 'C1008');

```

3. List Clients who stay in either 'Banglore or 'Manglore'
```sql
SELECT Cust_name FROM Cust_Master WHERE Cust_addr = 'Banglore' OR Cust_addr = 'Manglore';

```

4. Display name of customers & the product_name they have purchase
```sql
SELECT Cust_Master.Cust_name, Product.Product_name FROM Product INNER JOIN Orders ON Product.Order_no = Orders.Order_no INNER JOIN Cust_Master ON Orders.Cust_no = Cust_Master.Cust_no;

```

5. Create view View1 consisting of Cust_name, Product_name.
```sql
CREATE VIEW View1 AS SELECT Cust_Master.Cust_name, Product.Product_name FROM Product INNER JOIN Orders ON Product.Order_no = Orders.Order_no INNER JOIN Cust_Master ON Orders.Cust_no = Cust_Master.Cust_no;

SELECT * FROM View1;

```

6. Disply product_name and quantity purchase by each customer
```sql
SELECT Cust_Master.Cust_name, Product.Product_name, Orders.Qty_Ordered FROM Product INNER JOIN Orders ON Product.Order_no = Orders.Order_no INNER JOIN Cust_Master ON Orders.Cust_no = Cust_Master.Cust_no;

```

7. Perform different joint operation.

- INNER JOIN:

```sql
SELECT Cust_Master.Cust_name, Product.Product_name FROM Product INNER JOIN Orders ON Product.Order_no = Orders.Order_no INNER JOIN Cust_Master ON Orders.Cust_no = Cust_Master.Cust_no;

```

- OUTER LEFT JOIN:

```sql
SELECT Cust_Master.Cust_name, Orders.Order_no, Orders.Order_date FROM Cust_Master LEFT JOIN Orders ON Cust_Master.Cust_no = Orders.Cust_no;

```

- OUTER RIGHT JOIN:

```sql
SELECT Orders.Order_no, Orders.Order_date, Cust_Master.Cust_name FROM Orders RIGHT JOIN Cust_Master ON Orders.Cust_no = Cust_Master.Cust_no;

```

---
