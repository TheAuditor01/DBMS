# S6 - SQL Queries (in MySQL)

**Problem Statement:**
Consider following Relation
Employee(emp_id,employee_name,street,city)
Works(employee_name,company_name,salary)
Company(company_name,city)
Manages(employee_name,manager_name)
Create above tables with appropriate constraints like primary key,
foreign key, not null etc.
1. Change the city of employee working with InfoSys to ‘Bangalore’
2. Find the names of all employees who earn more than the average
salary of all employees of their company. Assume that all people
work for at most one company.
3. Find the names, street address, and cities of residence for all
employees who work for 'TechM' and earn more than $10,000.
4. Change name of table Manages to Management.
5. Create Simple and Unique index on employee table.
6. Display index Information

---

## Creating the database
```sql
CREATE DATABASE Companies2;
USE Companies2;

```

## Creating tables:

```sql
CREATE TABLE Employee (
  emp_id INT UNIQUE NOT NULL, -- can be set to auto increment using AUTO_INCREMENT
  employee_name VARCHAR(255),
  street VARCHAR(255),
  city VARCHAR(255),
  PRIMARY KEY (employee_name)
);

CREATE TABLE Works (
  employee_name VARCHAR(255),
  company_name VARCHAR(255),
  salary INT -- use FLOAT if you are feeling fancy and pay your employees in Paise
);

CREATE TABLE Company (
  company_name VARCHAR(255),
  city VARCHAR(255),
  PRIMARY KEY (company_name)
);

CREATE TABLE Manages (
  employee_name VARCHAR(255),
  manager_name VARCHAR(255)
);

```

## Declaring foreign keys

```sql
ALTER TABLE Works ADD FOREIGN KEY (employee_name) REFERENCES Employee (employee_name);
ALTER TABLE Works ADD FOREIGN KEY (company_name) REFERENCES Company (company_name);
ALTER TABLE Manages ADD FOREIGN KEY (employee_name) REFERENCES Employee (employee_name);


```

## Inserting data

```sql
INSERT INTO Employee VALUES
(1, 'Mehul', 'Street 42', 'Pune'),
(2, 'Himanshu', 'Street 74', 'Mumbai'),
(3, 'Gundeti', 'Street 14', 'Pune'),
(4, 'Salvi', 'Street 38', 'Pune'),
(5, 'Afan', 'Steet 98', 'Pune'),
(6, 'Jambo', 'Street 23', 'Mumbai');

INSERT INTO Company VALUES
('TCS', 'Pune'),
('Infosys', 'Mumbai'),
('TechM', 'Pune'),
('MEPA', 'Pune');

INSERT INTO Works VALUES
('Mehul', 'MEPA', 15000),
('Himanshu', 'TCS', 25000),
('Gundeti', 'TCS', 9000),
('Salvi', 'TechM', 8000),
('Afan', 'Infosys', 13000),
('Jambo', 'MEPA', 28000);


INSERT INTO Manages VALUES
('Mehul', 'Kalas'),
('Himanshu', 'Kshitij'),
('Gundeti', 'Macho'),
('Salvi', 'Kshitij'),
('Afan', 'Kalas'),
('Jambo', 'Macho');

```

## Queries

1. Change the city of employee working with InfoSys to ‘Bangalore’
```sql
UPDATE Company SET city = "Bangalore" WHERE company_name = "Infosys";
SELECT * FROM Company;

```

2. Find the names of all employees who earn more than the average salary of all employees of their company. Assume that all people work for at most one company.
```sql
SELECT employee_name, salary, company_name FROM Works as W WHERE salary > (SELECT AVG(salary) FROM Works WHERE company_name = W.company_name);

```

3. Find the names, street address, and cities of residence for all employees who work for 'TechM' and earn more than $10,000.
```sql
SELECT Employee.employee_name, street, city FROM Employee INNER JOIN Works ON Employee.employee_name = Works.employee_name WHERE salary > 10000;

```

4. Change name of table Manages to Management.
```sql
ALTER TABLE Manages RENAME TO Management;
SHOW TABLES;

```

5. Create Simple and Unique index on employee table.
```sql
-- Simple Index
CREATE INDEX emp_index ON Employee(employee_name);

-- Unique Index
CREATE UNIQUE INDEX emp_uniqueIndex ON Employee(emp_id);

```

6. Display index Information
```sql
SHOW INDEX FROM Employee;

```

---
