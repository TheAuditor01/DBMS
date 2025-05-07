# P5 - PL-SQL Block

**Problem Statement:** Write a PL/SQL Block to increase the salary of employees by 10% of existing salary, who are having salary less than average salary of organization, whenever such salary updates take place, a record for same is maintained in the increment_salary table.
emp(emp_no, salary)
increment_salary(emp_no, salary)

---

## Creating tables:
```sql
CREATE TABLE emp(
    emp_no NUMBER(14),
    salary NUMBER(14)
);

CREATE table increment_salary(
    emp_no NUMBER(14),
    salary NUMBER(14)
);

```

## Inserting values:
```sql
INSERT INTO emp VALUES (1, 1000);
INSERT INTO emp VALUES (2, 8000);
INSERT INTO emp VALUES (3, 2000);
INSERT INTO emp VALUES (4, 5000);
INSERT INTO emp VALUES (5, 7000);

```

## Procedure
```sql
DECLARE
    avg_salary NUMBER(14, 4);
BEGIN
    SELECT AVG(salary) INTO avg_salary FROM emp;
    FOR emp_record IN (select emp_no, salary FROM emp WHERE salary < avg_salary)
    LOOP
        insert into increment_salary(emp_no, salary) values (emp_record.emp_no, emp_record.salary);
        UPDATE emp SET salary = emp_record.salary * 1.10 WHERE emp_no = emp_record.emp_no;
    END LOOP;
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error: ' || SQLERRM);
end;
/

```

<details>
  <summary><strong>OUTPUT</strong>: After PL/SQL Block:</summary>
  emp:<br>
  EMP_NO	SALARY<br>
  1	1100<br>
  2	8000<br>
  3	2200<br>
  4	5000<br>
  5	7000<br>
  <br>
  increment_emp:<br>
  EMP_NO	SALARY<br>
  1	1000<br>
  3	2000<br>
</details>

---
