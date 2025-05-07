# S1 - SQL Queries (in MySQL)

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
1. Find the names of all branches in loan relation.
2. Find all loan numbers for loans made at ‘Wadia College’ Branch
with loan amount > 12000.
3. Find all customers who have a loan from bank. Find their
names,loan_no and loan amount.
4. List all customers in alphabetical order who have loan from
‘Wadia College’ branch.
5. Display distinct cities of branch.

---

## Creating the database
```sql
CREATE DATABASE Bank1;
USE Bank1;

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
('Salvi', 'Street 8', 'Pune');

INSERT INTO Account VALUES
(101, 'Lohegaon', 5500),
(102, 'PES', 4324),
(103, 'PES', 5467),
(104, 'Viman Nagar', 5433),
(105, 'Wadia College', 6462);

INSERT INTO Depositor VALUES
('Kalas', 101),
('Gundeti', 105);

INSERT INTO Loan VALUES
(201, 'Wadia College', 18000),
(202, 'PES', 8500),
(203, 'PES', 15000),
(204, 'Wadia College', 5322);

INSERT INTO Borrower VALUES
('Macho', 201),
('Mehul', 202),
('Himanshu', 203),
('Salvi', 204);

```

## Queries

1. Find the names of all branches in loan relation.
```sql
SELECT DISTINCT branch_name FROM Loan;

```

2. Find all loan numbers for loans made at ‘Wadia College’ Branch with loan amount > 12000.
```sql
SELECT loan_no FROM Loan WHERE branch_name = 'Wadia College' AND amount > 12000;

```

3. Find all customers who have a loan from bank. Find their names,loan_no and loan amount.
```sql
SELECT Borrower.cust_name, Borrower.loan_no, Loan.amount FROM Borrower INNER JOIN Loan ON Borrower.loan_no = Loan.loan_no;

```

4. List all customers in alphabetical order who have loan from ‘Wadia College’ branch.
```sql
SELECT cust_name FROM Borrower INNER JOIN Loan on Borrower.loan_no = Loan.loan_no WHERE Loan.branch_name = 'Wadia College' ORDER BY cust_name;

```

5. Display distinct cities of branch.
```sql
SELECT DISTINCT branch_city FROM Branch;

```
