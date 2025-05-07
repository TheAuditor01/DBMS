# S5 - SQL Queries (in MySQL)

**Problem Statement:**
Consider following Relation
Employee(emp_id,employee_name,street,city)
Works(employee_name,company_name,salary)
Company(company_name,city)
Manages(employee_name,manager_name)
Create above tables with appropriate constraints like primary key,
foreign key, not null etc.
1. Find the names of all employees who work for ‘TCS’.
2. Find the names and company names of all employees sorted in
ascending order of company name and descending order of employee
names of that company.
3. Change the city of employee working with InfoSys to ‘Bangalore’
4. Find the names, street address, and cities of residence for all
employees who work for 'TechM' and earn more than $10,000.
5. Add Column Asset to Company table.

---

## Creating the database
```sql
CREATE DATABASE Companies1;
USE Companies1;

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
(5, 'Afan', 'Steet 98', 'Pune');

INSERT INTO Company VALUES
('TCS', 'Pune'),
('Infosys', 'Mumbai'),
('TechM', 'Pune'),
('MEPA', 'Pune');

INSERT INTO Works VALUES
('Mehul', 'MEPA', 15000),
('Himanshu', 'TCS', 25000),
('Gundeti', 'TCS', 21500),
('Salvi', 'TechM', 11000),
('Afan', 'Infosys', 13000);


INSERT INTO Manages VALUES
('Mehul', 'Kalas'),
('Himanshu', 'Kshitij'),
('Gundeti', 'Macho'),
('Salvi', 'Kshitij'),
('Afan', 'Kalas');

```

## Queries

1. Find the names of all employees who work for ‘TCS’.
```sql
SELECT employee_name FROM Works WHERE company_name = "TCS";

```

2. Find the names and company names of all employees sorted in ascending order of company name and descending order of employee names of that company.
```sql
SELECT company_name, employee_name FROM Works ORDER BY company_name ASC, employee_name DESC;

```

3. Change the city of employee working with InfoSys to ‘Bangalore’
```sql
update Employee set city = "Banglore" where employee_name in (select employee_name from Works where company_name = "Infosys");
select * from Employee;

```

4. Find the names, street address, and cities of residence for all employees who work for 'TechM' and earn more than $10,000.
```sql
SELECT Employee.employee_name, Employee.street, Employee.city FROM Employee INNER JOIN Works ON Employee.employee_name = Works.employee_name WHERE Works.company_name = "TechM" AND Works.salary > 10000;

```


5. Add Column Asset to Company table.
```sql
ALTER TABLE Company ADD assets INT;
DESCRIBE Company;

```

---
