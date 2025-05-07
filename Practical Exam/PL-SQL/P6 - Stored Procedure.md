# P6 - Stored Procedure

**Problem Statement:** Write a Stored Procedure namely proc_Grade for the categorization of student. If marks scored by students in examination is <=1500 and marks>=990 then student will be placed in distinction category if marks scored are between 989 and 900 categories is first class, if marks 899 and 825 category is Higher Second Class. Write a PL/SQL block for using procedure created with above requirement.
Stud_Marks(name, total_marks),
Result (Roll,Name, Class)

---

## Creating tables
```plsql
CREATE TABLE Stud_Marks (
  name VARCHAR2(255),
  total_marks NUMBER,
  PRIMARY KEY (name)
);

CREATE TABLE Result (
  Roll NUMBER NOT NULL UNIQUE,
  Name VARCHAR2(255),
  Class VARCHAR2(255),
  FOREIGN KEY (Name) REFERENCES Stud_Marks (name)
);

```

## Inserting values
```plsql
INSERT INTO Stud_Marks VALUES ('Kalas', 1300);
INSERT INTO Stud_Marks VALUES ('Himanshu', 800);
INSERT INTO Stud_Marks VALUES ('Mehul', 950);
INSERT INTO Stud_Marks VALUES ('Gundeti', 875);

```

## Procedure
```plsql
CREATE OR REPLACE PROCEDURE proc_Grade (p_roll IN NUMBER, p_name IN VARCHAR2) IS
-- declare section
  marks NUMBER;
  nodata EXCEPTION;
BEGIN

  IF (p_name IS NULL) THEN
    raise nodata;
  END IF;

  SELECT total_marks INTO marks FROM Stud_Marks WHERE name = p_name;

  IF (marks >= 990 AND marks <= 1500) THEN
    DBMS_OUTPUT.PUT_LINE(p_name || ' has been placed in the distinction category.');
    INSERT INTO Result VALUES (p_roll, p_name, 'DISTINCTION');
  ELSIF (marks BETWEEN 900 AND 989) THEN
    DBMS_OUTPUT.PUT_LINE(p_name || ' has been placed in the first class category.');
    INSERT INTO Result VALUES (p_roll, p_name, 'FIRST CLASS');
  ELSIF (marks BETWEEN 825 AND 899) THEN
    DBMS_OUTPUT.PUT_LINE(p_name || ' has been placed in the higher second class category.');
    INSERT INTO Result VALUES (p_roll, p_name, 'HIGHER SECOND CLASS');
  ELSE
    DBMS_OUTPUT.PUT_LINE(p_name || ' has failed.');
    INSERT INTO Result VALUES (p_roll, p_name, 'FAIL');
  END IF;
EXCEPTION
  WHEN nodata THEN
    DBMS_OUTPUT.PUT_LINE('Please enter a name');
  WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('Error occurred. Error: ' || SQLERRM);
END proc_Grade;
/

```

## Calling procedure
```plsql
DECLARE
  p_roll NUMBER;
  p_name VARCHAR2(255);
BEGIN
  DBMS_OUTPUT.PUT_LINE('Enter roll number: ');
  p_roll := &p_roll;
  DBMS_OUTPUT.PUT_LINE('Enter name: ');
  p_name := '&p_name';

  proc_Grade(p_roll, p_name);
END;
/

```

---
