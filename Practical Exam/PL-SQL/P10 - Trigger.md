# P10 - Trigger

**Problem Statement:** Trigger: Write a after trigger for Insert, update and delete event considering following requirement:
Emp(Emp_no, Emp_name, Emp_salary)
a) Trigger should be initiated when salary tried to be inserted is less than Rs.50,000/-
b) Trigger should be initiated when salary tried to be updated for value less than Rs. 50,000/-
Also the new values expected to be inserted will be stored in new table Tracking(Emp_no,Emp_salary).

---

## Creating tables:
```sql
CREATE TABLE Emp(
    Emp_no NUMBER(14),
    Emp_name VARCHAR(255),
    Emp_salary NUMBER(14)
);

CREATE TABLE Tracking(
    Emp_no NUMBER(14),
    Emp_salary NUMBER(14)
);

```

## Trigger
```sql
CREATE OR REPLACE TRIGGER P10
AFTER INSERT OR UPDATE ON Emp
FOR EACH ROW
BEGIN
	IF inserting THEN
		IF (:New.Emp_salary < 50000) THEN
			insert into Tracking (Emp_no, Emp_salary) VALUES (:New.Emp_no, :New.Emp_salary);
			DBMS_OUTPUT.PUT_LINE('Inserting record with salary < 50000');
		END IF;
	ELSIF updating THEN
		IF (:New.Emp_salary < 50000) THEN
			UPDATE Tracking SET Emp_salary = :New.Emp_salary WHERE Emp_no = :Old.Emp_no;
			DBMS_OUTPUT.PUT_LINE('Updated value of salary < 50000 for a record');
		END IF;
	END IF;
END;
/

```

1. After performing insertion operation:
```sql
INSERT INTO Emp VALUES (1, 'Tanmay', 45000);
INSERT INTO Emp VALUES (2, 'Rajesh', 35000);

```

<details>
  <summary>Output</summary>
  1 row(s) inserted.<br>
  Inserting record with salary < 50000<br>
<br>
  1 row(s) inserted.<br>
  Inserting record with salary < 50000<br>
<br>
  Tracking:<br>
  EMP_NO	EMP_SALARY<br>
  1	45000<br>
  2	35000<br>

</details>

2. After performing update operation:
```sql
UPDATE Emp SET Emp_salary = 43000 WHERE Emp_no = 1;

```
<details>
  <summary>Output</summary>
  1 row(s) updated.<br>
  Updated value of salary < 50000 for a record<br>
  <br>
  Tracking:<br>
  EMP_NO	EMP_SALARY<br>
  1	43000<br>
  2	35000<br>

</details>

---
