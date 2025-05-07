# P7 - Age_calc function

**Problem statement:** Create a stored function titled 'Age_calc'. Accept the date of birth of a person as a parameter. Calculate the age of the person in years, months and days e.g. 3 years, 2months, 10 days. Return the age in years directly (with the help of Return statement). The months and days are to be returned indirectly in the form of OUT parameters.

## Creating function

```sql
CREATE OR REPLACE FUNCTION Age_calc(
    dob IN DATE,
    f_month OUT NUMBER,
    f_day OUT NUMBER
)
RETURN NUMBER
IS
	current_date DATE := SYSDATE;
	year_diff NUMBER;
	month_diff NUMBER;
	day_diff NUMBER;
BEGIN
	year_diff := EXTRACT(YEAR FROM current_date) - EXTRACT(YEAR FROM dob);
	month_diff := EXTRACT(MONTH FROM current_date) - EXTRACT(MONTH FROM dob);
	day_diff := EXTRACT(DAY FROM current_date) - EXTRACT(DAY FROM dob);

	IF (day_diff < 0) THEN
		month_diff := month_diff - 1;
		day_diff := day_diff + EXTRACT(DAY FROM last_day(add_months(SYSDATE, -1)));
	END IF;

	IF (month_diff < 0) THEN
		year_diff := year_diff - 1;
		month_diff := month_diff + 12;
	END IF;

	f_month := month_diff;
	f_day := day_diff;

	RETURN year_diff;
END;
/

```

## Calling function

```sql
DECLARE
    years NUMBER;
    months NUMBER;
    days NUMBER;
BEGIN
    years := Age_calc(TO_DATE('2003-12-02', 'YYYY-MM-DD'), months, days);
    DBMS_OUTPUT.PUT_LINE('Age: ' || years || ' Years ' || months || ' Months ' || days || ' Days.');
END;
/

```

---
