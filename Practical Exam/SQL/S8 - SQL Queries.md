# S8 - SQL Queries (in MySQL)

**Problem Statement:**
Consider following Relation:
Companies (comp_id, name, cost, year)
Orders (comp_id, domain, quantity)
Execute the following query:
1. Find names, costs, domains and quantities for companies using
inner join.
2. Find names, costs, domains and quantities for companies using
left outer join.
3. Find names, costs, domains and quantities for companies using
right outer join.
4. Find names, costs, domains and quantities for companies using
Union operator.
5. Create View View1 by selecting both tables to show company name
and quantities.
6. Create View View2 by selecting any two columns and perform
insert update delete operations.
7. Display content of View1, View2.

---

## Creating the database
```sql
CREATE DATABASE Store2;
USE Store2;

```

## Creating tables:

```sql
CREATE TABLE Companies (
  comp_id INT,
  name VARCHAR(255),
  cost INT,
  year INT,
  PRIMARY KEY (comp_id)
);

CREATE TABLE Orders (
  comp_id INT,
  domain VARCHAR(255),
  quantity INT,
  FOREIGN KEY (comp_id) REFERENCES Companies (comp_id)
);

```

## Inserting data

```sql
INSERT INTO Companies VALUES
(1, 'MEPA', 40500, 2024),
(2, 'Wayne Industries', 950000, 2000),
(3, 'Oscorp', 64600, 2013),
(4, 'Lex Corp', 28500, 2001),
(5, 'Vought', 77335, 2020);

INSERT INTO Orders VALUES
(1, 'Healthcare', 45),
(2, 'Kevlar', 30),
(3, 'Goblin masks', 62),
(4, 'Haircare', 23),
(5, 'Spandex', 9);

```

## Queries

1. Find names, costs, domains and quantities for companies using inner join.
```sql
SELECT name, cost, domain, quantity FROM Companies INNER JOIN Orders ON Companies.comp_id = Orders.comp_id;
SELECT DISTINCT name, cost, domain, quantity FROM Companies, Orders;

```

2. Find names, costs, domains and quantities for companies using left outer join.
```sql
SELECT name, cost, domain, quantity FROM Companies LEFT JOIN Orders ON Companies.comp_id = Orders.comp_id;

```

3. Find names, costs, domains and quantities for companies using right outer join.
```sql
SELECT name, cost, domain, quantity FROM Companies RIGHT JOIN Orders on Companies.comp_id = Orders.comp_id;

```

4. Find names, costs, domains and quantities for companies using Union operator.
```sql
SELECT name AS info, cost AS value FROM Companies UNION SELECT domain AS info, quantity AS value FROM Orders;

```

5. Create View View1 by selecting both tables to show company name and quantities.
```sql
CREATE VIEW View1 AS SELECT name, quantity FROM Companies INNER JOIN Orders ON Companies.comp_id = Orders.comp_id;
SELECT * FROM View1;

```

6. Create View View2 by selecting any two columns and perform insert update delete operations.
```sql
-- Creating view
CREATE VIEW View2 AS SELECT Companies.comp_id, domain FROM Companies INNER JOIN Orders ON Companies.comp_id = Orders.comp_id;
SELECT * FROM View2;

-- Insert operation
INSERT INTO Companies VALUES (6, 'Stark Industries', 54322, 2012);
INSERT INTO Orders VALUES (6, 'Vibranium', 66);
SELECT * FROM View2;

-- Update operation
UPDATE View2 SET domain = 'Iridium' WHERE comp_id = 6;
SELECT * FROM View2;

-- Delete operation
DELETE FROM Orders WHERE comp_id = 8;
DELETE FROM Companies WHERE comp_id = 8;
SELECT * FROM View2;

```

7. Display content of View1, View2.
```sql
SELECT * FROM View1;
SELECT * FROM View2;

```

---
