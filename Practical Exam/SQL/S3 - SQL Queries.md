# S3 - SQL Queries (in MySQL)

**Problem Statement:**
Consider following Relation
Account (Acc_no, branch_name,balance)
Branch(branch_name,branch_city,assets)
Customer(cust_name,cust_street,cust_city)
Depositor(cust_name,acc_no)
Loan(loan_no,branch_name,amount)
Borrower(cust_name,loan_no)
Create above tables with appropriate constraints like primary key,
foreign key, not null etc.
1. Find the branches where average account balance > 15000.
2. Find number of tuples in customer relation.
3. Calculate total loan amount given by bank.
4. Delete all loans with loan amount between 1300 and 1500.
5. Find the average account balance at each branch
6. Find name of Customer and city where customer name starts with
Letter P.

---

## Creating the database
```sql
CREATE DATABASE Bank3;
USE Bank3;

```

## Creating tables:

```sql
CREATE TABLE Account (
  acc_no INT,
  branch_name VARCHAR(255),
  balance INT,
  PRIMARY KEY (acc_no)
);

CREATE TABLE Branch (
  branch_name VARCHAR(255),
  branch_city VARCHAR(255),
  assets INT,
  PRIMARY KEY (branch_name)
);

CREATE TABLE Customer (
  cust_name VARCHAR(255),
  cust_street VARCHAR(255),
  cust_city VARCHAR(255),
  PRIMARY KEY (cust_name)
);

CREATE TABLE Depositor (
  cust_name VARCHAR(255),
  acc_no INT
);

CREATE TABLE Loan (
  loan_no INT,
  branch_name VARCHAR(255),
  amount INT,
  PRIMARY KEY (loan_no)
);

CREATE TABLE Borrower (
  cust_name VARCHAR(255),
  loan_no INT
);

```

## Declaring foreign keys

```sql
ALTER TABLE Account ADD FOREIGN KEY (branch_name) REFERENCES Branch (branch_name);
ALTER TABLE Depositor ADD FOREIGN KEY (cust_name) REFERENCES Customer (cust_name);
ALTER TABLE Depositor ADD FOREIGN KEY (acc_no) REFERENCES Account (acc_no);
ALTER TABLE Loan ADD FOREIGN KEY (branch_name) REFERENCES Branch (branch_name);
ALTER TABLE Borrower ADD FOREIGN KEY (cust_name) REFERENCES Customer (cust_name);
ALTER TABLE Borrower ADD FOREIGN KEY (loan_no) REFERENCES Loan (loan_no);

```

## Inserting data

```sql
INSERT INTO Branch VALUES
('Wadia College', 'Pune', 50000),
('PES', 'Pune', 65000),
('Lohegaon', 'Pune', 350000),
('Viman Nagar', 'Pune', 850000);

INSERT INTO Customer VALUES
('Kalas', 'Street 12', 'Pune'),
('Himanshu', 'Street 15', 'Pune'),
('Mehul', 'Street 29', 'Pune'),
('Macho', 'Street 59', 'Mumbai'),
('Gundeti', 'Street 40', 'Mumbai'),
('Salvi', 'Street 8', 'Pune'),
('Pintu', 'Street 55', 'Ahemadnagar'),
('Piyush', 'Street 21', 'Assam');

INSERT INTO Account VALUES
(101, 'Lohegaon', 67000),
(102, 'PES', 4324),
(103, 'PES', 54670),
(104, 'Viman Nagar', 5433),
(105, 'Wadia College', 6462);

INSERT INTO Depositor VALUES
('Kalas', 101),
('Macho', 104),
('Gundeti', 105),
('Salvi', 105);

INSERT INTO Loan VALUES
(201, 'Wadia College', 1800),
(202, 'PES', 8500),
(203, 'PES', 15000),
(204, 'Wadia College', 5322),
(205, 'Viman Nagar', 1300),
(206, 'Lohegaon', 1450);

INSERT INTO Borrower VALUES
('Macho', 201),
('Mehul', 202),
('Himanshu', 203),
('Salvi', 204);

```

## Queries

1. Find the branches where average account balance > 15000.
```sql
SELECT branch_name FROM Account GROUP BY branch_name HAVING AVG(balance) > 15000 ;

```

2. Find number of tuples in customer relation.
```sql
SELECT COUNT(*) FROM Customer;

```

3. Calculate total loan amount given by bank.
```sql
SELECT SUM(amount) FROM Loan;

```

4. Delete all loans with loan amount between 1300 and 1500.
- Delete the dependent records first(due to foreign key constraints):
```sql
DELETE FROM Borrower
WHERE loan_no IN (
  SELECT loan_no
  FROM Loan
  WHERE amount BETWEEN 1300 AND 1500
);
```
- Now delete the Loan table Records:
```sql
DELETE FROM Loan WHERE amount BETWEEN 1300 AND 1500;

```

5. Find the average account balance at each branch
```sql
SELECT branch_name, AVG(balance) FROM Account GROUP BY branch_name;

```

6. Find name of Customer and city where customer name starts with letter P.
```sql
SELECT cust_name, cust_city FROM Customer WHERE cust_name LIKE "P%";

```

---
