# C1 - MongoDB Connectivity

**Problem Statement:** Write a program to implement MongoDB database connectivity with PHP /python /Java Implement Database navigation CRUD operations (add, delete, edit etc.)

---

## Pre-requisites (with installation command for Ubuntu):
1. Python3 - `sudo apt install python3`
2. pip - `sudo apt install python3-pip`
3. pymongo -  `pip3 install pymongo` OR `pip3 install pymongo --break-system-packages` (installs it system-wide)

## Instructions

1. First, open the Terminal and log in to MongoDB:
```shell
mongo

```

2. Now, in Mongo we need to initialize the database and tables:
```json
// Create database
use connectMe;
// Create collection
db.createCollection("students");
// exit
exit

```

> We need to `exit` from Mongo shell since the work there is done for now.

3. Create a new file `connectMongo.py` with the following contents:
```python
from pymongo import MongoClient

client = MongoClient("mongodb://127.0.0.1:27017")
database = client.connectMe
collection = database.students

collection.insert_one({"roll":"21", "name":"Stewie"}) # insert operation
print("Inserted")
collection.insert_one({"roll":"22", "name":"Foxy"}) # insert operation
print("Inserted")
collection.update_one({"roll": "21"}, {"$set": {"roll": "20"}}) # update operation
print("Updated")
collection.delete_one({"roll": "22"})
print("Deleted")

client.close()

```

4. Open Terminal and run the above program:
```shell
python3 connectMongo.py
```

> [!NOTE]
> This step assume you have stored the file in home directory or you know how to change the current working directory.

5. That's it! You can view the changes in Mongo by logging in using `mongo` and viewing the table:
```json
USE connectMe;
db.students.find();

```

> [!Tip]
> This guide assumes your `mongo` shell does not have any password for root user.

> [!WARNING]
> Never use `root` as password in production. Use a strong password. This guide uses `root` as password for simplicity and educational purposes.

---
