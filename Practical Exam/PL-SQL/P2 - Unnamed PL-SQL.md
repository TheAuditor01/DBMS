# P2 - Unamed PL-SQL

**Problem Statement:**
Write an Unnamed PL/SQL of code for the following requirements: -
Schema:
Borrower (Rollin, Name, DateofIssue, NameofBook, Status)
Fine (Roll_no,Date,Amt)
Accept roll_no & name of book from user.
Check the number of days (from date of issue).
1. If days are between 15 to 30 then fine amounts will be Rs 5 per
day.
2. If no. of days>30, per day fine will be Rs 50 per day & for days
less than 30, Rs. 5 per day.
3. After submitting the book, status will change from I to R.
4. If condition of fine is true, then details will be stored into
fine table.

---

## Creating tables
```plsql
CREATE TABLE Borrower (
  rollin NUMBER,
  Name VARCHAR2(255),
  DateofIssue DATE,
  NameofBook VARCHAR2(255),
  Status VARCHAR2(255),
  PRIMARY KEY (rollin)
);

CREATE TABLE Fine (
  Roll_no NUMBER,
  DateofReturn DATE,
  Amt NUMBER,
  FOREIGN KEY (Roll_no) REFERENCES Borrower (rollin)
);

```

> [!WARNING]
> Notice inconsistent naming for columns? We're just doing it by the books. Blame the one who made these [problem statements](https://git.kska.io/sppu-te-comp-content/DatabaseManagementSystems/src/branch/main/Practical/Practical%20Exam/DBMSL%20-%20Problem%20Statements%20for%20Practical%20Exam%20%28November%202024%29.pdf).

## Inserting data
```plsql
INSERT INTO Borrower VALUES (1, 'Kalas', TO_DATE('2024-10-19', 'YYYY-MM-DD'), 'DBMS', 'I');
INSERT INTO Borrower VALUES (2, 'Himanshu', TO_DATE('2024-11-01', 'YYYY-MM-DD'), 'IOT', 'I');
INSERT INTO Borrower VALUES (3, 'Mepa', TO_DATE('2024-10-29', 'YYYY-MM-DD'), 'TOC', 'I');
INSERT INTO Borrower VALUES (4, 'Jambo', TO_DATE('2024-10-20', 'YYYY-MM-DD'), 'CNS', 'I');

```

## Procedure
```plsql
DECLARE
  p_roll NUMBER;
  p_book VARCHAR2(255);
  p_issueDate DATE;
  totalDays INT;
  fineAmt INT;
  nodata EXCEPTION;
BEGIN
  DBMS_OUTPUT.PUT_LINE('Enter roll number: ');
  p_roll := &p_roll; -- specify value directly for Live SQL. Eg.: p_roll := 1;
  DBMS_OUTPUT.PUT_LINE('Enter book name: ');
  p_book := '&p_book'; -- specify value directly for Live SQL. Eg.: p_roll := 'DBMS';

  IF (p_roll <= 0) THEN
    RAISE nodata;
  END IF;

  SELECT DateofIssue INTO p_issueDate FROM Borrower WHERE rollin = p_roll AND NameofBook = p_book;
  SELECT TRUNC(SYSDATE) - p_issueDate INTO totalDays FROM dual;

  IF (totalDays > 30) THEN
    fineAmt := (15 * 5) + ((totalDays - 15) * 50); -- Rs 5 for first 15 days, Rs 50 for remaining
    DBMS_OUTPUT.PUT_LINE('Roll no. ' || p_roll || ' has been fined Rs. ' || fineAmt);
    INSERT INTO Fine VALUES (p_roll, SYSDATE, fineAmt);
    UPDATE Borrower SET Status = 'R' WHERE Rollin = p_roll AND NameofBook = p_book;
  ELSIF (totalDays BETWEEN 15 AND 30) THEN
    fineAmt := totalDays * 5;
    DBMS_OUTPUT.PUT_LINE('Roll no. '|| p_roll || ' has been fined Rs. ' || fineAmt);
    INSERT INTO Fine VALUES (p_roll, SYSDATE, fineAmt);
    UPDATE Borrower SET Status = 'R' WHERE Rollin = p_roll AND NameofBook = p_book;
  ELSE
    fineAmt := 0;
    DBMS_OUTPUT.PUT_LINE('No fine for roll no. '|| p_roll);
    INSERT INTO Fine VALUES (p_roll, SYSDATE, fineAmt);
    UPDATE Borrower SET Status = 'R' WHERE Rollin = p_roll AND NameofBook = p_book;
  END IF;
EXCEPTION
  WHEN nodata THEN
    DBMS_OUTPUT.PUT_LINE('Please enter a valid roll number.');
  WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('Something went wrong. Please check input data.');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('Error occured. Error: ' || SQLERRM);
END;
/

```

---
