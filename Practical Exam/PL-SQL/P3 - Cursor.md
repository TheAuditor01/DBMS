# P3 - Cursor

**Problem Statement:** Write a PL/SQL block of code using Cursor that will merge the data available in the newly created table N_Roll Call with the data available in the table O_RollCall. If the data in the first table already exist in the second table, then that data should be skipped.

---

## Creating tables
```plsql
CREATE TABLE O_RollCall (
  roll NUMBER NOT NULL,
  name VARCHAR2(255),
  class VARCHAR2(255)
);

CREATE TABLE N_RollCall (
  roll NUMBER NOT NULL,
  name VARCHAR2(255),
  class VARCHAR2(255)
);

```

## Inserting values
```plsql
INSERT INTO O_RollCall VALUES (2, 'Eddie', 'COMP-2');
INSERT INTO O_RollCall VALUES (3, 'Foxy', 'COMP-1');
INSERT INTO O_RollCall VALUES (5, 'Stomp', 'COMP-3');

INSERT INTO N_RollCall VALUES (1, 'Stewie', 'COMP-1');
INSERT INTO N_RollCall VALUES (2, 'Eddie', 'COMP-2');
INSERT INTO N_RollCall VALUES (3, 'Foxy', 'COMP-1');
INSERT INTO N_RollCall VALUES (4, 'Lara', 'COMP-3');
INSERT INTO N_RollCall VALUES (5, 'Stomp', 'COMP-3');

```

## Procedure
```plsql
DECLARE
  p_roll NUMBER;
  p_name VARCHAR2(255);
  p_class VARCHAR2(255);
  CURSOR c1 IS SELECT * FROM N_RollCall WHERE roll NOT IN (SELECT roll FROM O_RollCall);
BEGIN
  OPEN c1;
    LOOP
      FETCH c1 INTO p_roll, p_name, p_class;
      INSERT INTO O_RollCall VALUES (p_roll, p_name, p_class);
      EXIT WHEN c1%NOTFOUND;
      DBMS_OUTPUT.PUT_LINE('Inserted ' || p_name || ' having roll no. ' || p_roll || ' in class ' || p_class || '.');
    END LOOP;
  CLOSE c1;
END;
/

```

---
