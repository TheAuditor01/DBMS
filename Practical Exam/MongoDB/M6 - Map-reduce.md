# M6 - Map-reduce

**Problem statement:**
Design MongoDB database and perform following Map reduce operation:
Create Employee collection by considering following Fields:
i. Name: Embedded Doc (FName, LName)
ii. Company Name: String
iii. Salary: Number
iv. Designation: String
v. Age: Number
vi. Expertise: Array
vii. DOB: String or Date
viii. Email id: String
ix. Contact: String
x. Address: Array of Embedded Doc (PAddr, LAddr)
Execute the following query:

---

## Creating database
```mongo
use mapred;

```

## Inserting data:
```mongo
db.emp.insertMany([
  {
    Name: {FName: "Tanmay", LName: "Machkar"},
    Company: "TCS",
    Salary: 40000,
    Designation: "Tester",
    Age: 25,
    Expertise: ['Mongodb','Mysql','Cassandra'],
    DOB: new Date("2003-12-02"),
    Email: "xyz@gmail.com",
    Contact: 1234567890,
    Address: [{PAddr: {City: "Pune", Rd: "Bharatmata"}}, {LAddr: {Pin_code: 411001}}]
  },
  {
    Name: {FName: "Rajesh", LName: "Machkar"},
    Company: "Infosys",
    Salary: 50000,
    Designation: "Programmer",
    Age: 29,
    Expertise: ['Mongodb','Mysql'],
    DOB: new Date("1990-12-02"),
    Email: "xy@gmail.com",
    Contact: 1234567809,
    Address: [{PAddr: {City: "Pune", Rd: "JM"}}, {LAddr: {Pin_code: 411047}}]
  },
  {
    Name: {FName: "Tejas", LName: "Machkar"},
    Company: "TCS",
    Salary: 20000,
    Designation: "Developer",
    Age: 35,
    Expertise: ['Mongodb'],
    DOB: new Date("2000-11-22"),
    Email: "x@gmail.com",
    Contact: 1234567089,
    Address: [{PAddr: {City: "Mumbai", Rd: "Airport"}}, {LAddr: {Pin_code: 411030}}]
  },
  {
    Name: {FName: "Deepali", LName: "Machkar"},
    Company: "Persistent",
    Salary: 40000,
    Designation: "Tester",
    Age: 30,
    Expertise: ['Mysql'],
    DOB: new Date("1978-02-12"),
    Email: "y@gmail.com",
    Contact: 1234506789,
    Address: [{PAddr: {City: "Phaltan", Rd: "Highway"}}, {LAddr: {Pin_code: 411012}}]
  },
  {
    Name: {FName: "Om", LName: "Deokar"},
    Company: "Persistent",
    Salary: 25000,
    Designation: "Programmer",
    Age: 39,
    Expertise: ['Cassandra'],
    DOB: new Date("2007-03-15"),
    Email: "z@gmail.com",
    Contact: 1234567890,
    Address: [{PAddr: {City: "Jaipur", Rd: "Pink"}}, {LAddr: {Pin_code: 411056}}]
  },
])
```

## Queries

1. Display the total salary of per company.
```mongo
db.emp.mapReduce(
  function(){
    emit(this.Company, this.Salary);
  },
  function(key, values){
    return Array.sum(values);
  },
  { out: "query1"}
)
db.query1.find()

```

2. Display the total salary of company Name:"TCS".
```mongo
db.emp.mapReduce(
  function(){
    if(this.Company == "TCS"){
      emit(this.Company, this.Salary);
    }
  },
  function(key, values){
    return Array.sum(values);
  },
  { out: "query2"}
)
db.query2.find()

```

3. Return the average salary of company whose address is “Pune".
```mongo
db.emp.mapReduce(
  function(){
    if(this.Address[0].PAddr.City === "Pune"){
      emit(this.Company, this.Salary);
    }
  },
  function(key, values){
    return Array.avg(values);
  },
  { out: "query3"}
)
db.query3.find()

```

4. Display total count for “City=Pune”.
```mongo
db.emp.mapReduce(
  function(){
    if(this.Address[0].PAddr.City === "Pune"){
      emit("Count", 1);
    }
  },
  function(key, values){
    return Array.sum(values);
  },
  { out: "query4"}
)
db.query4.find()

```

5. Return count for city pune and age greater than 40.
```mongo
db.emp.mapReduce(
  function(){
    if(this.Address[0].PAddr.City === "Pune" && this.Age > 25){
      emit("Count", 1);
    }
  },
  function(key, values){
    return Array.sum(values);
  },
  { out: "query5"}
)
db.query5.find()

```

---

