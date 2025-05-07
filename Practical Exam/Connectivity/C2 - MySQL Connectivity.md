# C2 - MySQL Connectivity

**Problem Statement:** Implement MYSQL/Oracle database connectivity with PHP /python /Java Implement Database navigation operations (add, delete, edit,).

---

## Pre-requisites (with installation command for Ubuntu):

1. Python3 - `sudo apt install python3`
2. pip - `sudo apt install python3-pip`
3. mysql-connector -  `pip3 install mysql-connector` OR `pip3 install mysql-connector --break-system-packages` (installs it system-wide)

## Instructions

1. First, open the Terminal and log in to MySQL:
```shell
mysql -u root -p

```

> [!NOTE]
> Usually the password in our labs is `root`. If you don't know the password, try running `sudo mysql`. This will run the `mysql` command as root.

2. Whether you logged in as root or not, run this command to set the password for `root` user in mysql which is required for connecting to the database:
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
FLUSH PRIVILEGES;
```

3. Now, in MySQL we need to initialize the database and tables:
```sql
-- Creating the database
CREATE DATABASE connectMe;
USE connectMe;

-- Creating a table
CREATE TABLE students (
  roll INT,
  name VARCHAR(255)
);

-- Exit
exit

```

> We need to `exit` from MySQL shell since the work there is done for now.

5. Create a new file `connectSQL.py` with the following contents:
```python
import mysql.connector
# Connecting to MySQL database
db = mysql.connector.connect(
  host="localhost",
  user="root",
  password="root", # Password of root user we set in step 2
  database="connectMe" # Name of the database we created in step 3
)

Cursor = db.cursor()

sqlInsert = "INSERT INTO students (roll, name) VALUES (21, 'Stewie')" # insert operation
Cursor.execute(sqlInsert)
print(Cursor.rowcount, "record added to database.")

Cursor.execute(sqlInsert)
sqlInsert = "INSERT INTO students (roll, name) VALUES (22, 'Foxy')" # insert operation
print(Cursor.rowcount, "record added to database.")

sqlUpdate = "UPDATE students SET roll = 21  WHERE roll = 20" # update operation
Cursor.execute(sqlUpdate)
print(Cursor.rowcount, "record updated.")

sqlDelete = "DELETE FROM students WHERE roll = 20" # delete operation
Cursor.execute(sqlDelete)
print(Cursor.rowcount, "record deleted.")

db.commit()
Cursor.close()
db.close()

```

6. Open Terminal and run the above program:
```shell
python3 connectSQL.py

```

> [!NOTE]
> This step assume you have stored the file in home directory or you know how to change the current working directory.

7. That's it! You can view the changes in MySQL by logging in using `mysql -u root -p` (Password: `root`) and viewing the table:
```sql
USE connectMe;
SELECT * FROM students;

```

> [!WARNING]
> Never use `root` as password in production. Use a strong password. This guide uses `root` as password for simplicity and educational purposes.

---
