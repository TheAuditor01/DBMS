# P4 - PL/SQL block

**Problem Statement:** Write a PL/SQL block for following requirements and handle the exceptions. Roll no. of students will be entered by the user. Attendance of roll no. entered by user will be checked in the Stud table. If attendance is less than 75% then display the message “Term not granted” and set the status in stud table as “Detained”. Otherwise display message “Term granted” and set the status in stud table as “Not Detained”. Student (Roll, Name, Attendance, Status)

---

## Creating table

```sql
CREATE TABLE Student(
    Roll NUMBER(14),
    Name VARCHAR(255),
    Attendance NUMBER(14),
    Status VARCHAR(255)
);

```

## Inserting data

```sql
INSERT INTO Student VALUES (1, 'Tanmay', 76, NULL);
INSERT INTO Student VALUES (2, 'Rajesh', 80, NULL);
INSERT INTO Student VALUES (3, 'Tejas', 88, NULL);
INSERT INTO Student VALUES (4, 'Machkar', 35, NULL);
INSERT INTO Student VALUES (5, 'Jayashree', 74, NULL);

```

## Procedure

```sql
DECLARE
    p_att Student.Attendance%type;
    p_roll Student.Roll%type;
    nodata EXCEPTION;
BEGIN
    p_roll := &p_roll; -- specify value directly for Live SQL. Eg.: p_roll := 1;

    IF (p_roll < 0) THEN
        raise nodata;
    END IF;

    SELECT Attendance INTO p_att FROM Student WHERE Roll = p_roll;

    IF (p_att < 75) THEN
        DBMS_OUTPUT.PUT_LINE('Term not granted to roll no. ' || p_roll);
        UPDATE Student SET Status = 'Detained' WHERE Roll = p_roll;
    ELSIF (p_att >= 75) THEN
        DBMS_OUTPUT.PUT_LINE('Term granted to roll no. ' || p_roll);
        UPDATE Student SET Status = 'Not Detained' WHERE Roll = p_roll;
    END IF;
EXCEPTION
    WHEN nodata THEN
        DBMS_OUTPUT.PUT_LINE('Please enter a valid roll number.');
		WHEN OTHERS THEN
	      DBMS_OUTPUT.PUT_LINE('Error occured. Error: ' || SQLERRM);
END;
/

```

<details>
  <summary>Output</summary>
  After entering every Roll no:
  ROLL	NAME	  ATTENDANCE	STATUS<br>
  1	Tanmay	  76		Not Detained<br>
  2	Rajesh	  80		Not Detained<br>
  3	Tejas	  88		Not Detained<br>
  4	Machkar	  35		Detained<br>
  5	Jayashree 74		Detained<br>
</details>

---
