# P9 - Trigger

**Problem Statement:** Trigger: Create a row level trigger for the CUSTOMERS table that would fire INSERT or UPDATE or DELETE operations performed on the CUSTOMERS table. This trigger will display the salary difference between the old values and new values.

---

## Creating table:

```sql
CREATE TABLE Customers(
    name VARCHAR(255),
    id NUMBER(14),
    salary NUMBER(14)
);

```

## Procedure

```sql
CREATE OR REPLACE TRIGGER P
AFTER INSERT OR UPDATE OR DELETE ON Customers
FOR EACH ROW
DECLARE
    diff NUMBER(14);
BEGIN
    IF INSERTING THEN
        DBMS_OUTPUT.PUT_LINE('New salary with value inserted: ' || :NEW.salary);
    ELSIF UPDATING THEN
        if :New.salary > :old.salary then diff := :New.salary - :Old.salary;
	elsif :New.salary < :old.salary then diff := :Old.salary - :New.salary;
	end if;
        DBMS_OUTPUT.PUT_LINE('Salary difference: ' || diff);
    ELSIF DELETING THEN
        DBMS_OUTPUT.PUT_LINE('Salary with value deleted: ' || :OLD.salary);
    END IF;
END;
/

```

## Queries

1. Insert operation:

```sql
INSERT INTO Customers VALUES ('Tanmay', 1, 20000);
INSERT INTO Customers VALUES ('Rajesh', 2, 30000);

```

<details>
  <summary>Output</summary>
    1 row(s) inserted.<br>
    New salary with value inserted: 20000<br>
    1 row(s) inserted.<br>
    New salary with value inserted: 30000<br>
</details>

2. Update operation:

```sql
UPDATE Customers SET salary = 10000 WHERE id = 1;
UPDATE Customers SET salary = 35000 WHERE id = 2;

```

<details>
  <summary>Output</summary>
    1 row(s) updated.<br>
    Salary difference: 10000<br>
    1 row(s) updated.<br>
    Salary difference: 5000<br>
</details>


3. Delete operation:

```sql
DELETE FROM Customers WHERE id = 1;
DELETE FROM Customers WHERE id = 2;

```

<details>
  <summary>Output</summary>
    1 row(s) deleted.<br>
    Salary with value deleted: 10000<br>
    1 row(s) deleted.<br>
    Salary with value deleted: 35000<br>
</details>

---
