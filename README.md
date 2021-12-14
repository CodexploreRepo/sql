# Database
ISSS615 - Data Management
# Table of contents
- [Table of contents](#table-of-contents)
- [1. Introduction to Database](#1-introduction-to-database)
   - [1.1. Important Concepts](#11-important-concepts)
   - [1.2. Database Management System](#12-database-management-system)
   - [1.3 Types of DB](#13-types-of-db)
   - [1.4. Life Cycle of a Database](#14-life-cycle-of-a-database)
- [2. SQL commands](#2-sql-commands)
   - [2.1. Basic SQL commands](#21-basic-sql-commands)
   - [2.2. Filtering Data](#22-filtering-data)  
   - [2.3. Joining Tables](#23-joining-tables)
- [3. Frequently-used SQL commands](#3-frequently-used-sql-commands)
- [4. SQL Function](#4-sql-function)
- [Cartesian Product](#cartesian-product)
- [LeetCode](#leetcode)
- [Appendix](#appendix)
   - [A1. Relational Database: PostgreSQL](#A1-relational-database-postgresql) 


# 1. Introduction to Database
## 1.1. Important Concepts
### 1.1.1. Keywords
- `Data`: stored representations of meaningful objects and events
   - Structured: numbers, text, dates
   - Unstructured: images, video, documents
- `Information`: data processed to increase knowledge in the person using the data
- `Metadata`: data that describes the properties and context of user data
- `Database`: organized collection of logically related data

<p align="center"><img height="180" alt="Screenshot 2021-12-14 at 21 06 31" src="https://user-images.githubusercontent.com/64508435/146015444-e63bc1c2-d243-4aae-a0ef-47957b26c184.png"></p>


### 1.1.2. Databases vs. Data Warehouses
- Data Warehouse: An integrated decision support database whose content is derived from the various operational databases. 
   - Support information processing by providing a solid platform of consolidated, historical data for analysis 

<p align="center"><img width="849" alt="Screenshot 2021-12-14 at 21 06 31" src="https://user-images.githubusercontent.com/64508435/146013652-05d3ef69-bdc5-44e5-85b4-b456b81e7b98.png"></p>

## 1.2. Database Management System
- `Database Management System` (DBMS): a data storage and retrieval system which permits data to be stored non-redundantly while making it appear to the user as if the data is well-integrated
   - Applications cannot directly touch your data, only DBMS can do that
   - **Benefit**: Program – data independence. When a program interact with data, the program don’t need to care where/how the data is stored. &#8594; application development is faster and less costly
 
<p align="center"><img width="350" alt="Screenshot 2021-12-14 at 21 24 04" src="https://user-images.githubusercontent.com/64508435/146016554-f4175862-5e90-422e-856d-5b7afb8502b4.png"></p>

### 1.2.1. DBMS Market Place
- **Enterprise DBMS**
   - Oracle: dominates in Unix; strong in Windows
   - Microsoft SQL Server: strong in Windows
   - DB2: strong in mainframe environment
- **Desktop DBMS**: Access
- **Open Source DBMS**: MySQ: (Apple, Sun, SAP, Dell, Intel, Novell)

[(Back to top)](#table-of-contents)

## 1.3 Types of DB
<p align="center"><img src="https://user-images.githubusercontent.com/64508435/92753545-93f61280-f3bc-11ea-81d7-77181f2105b0.png" alt="drawing" height="250"/></p>

1. **Relational DB**: consists of 2 or more tables with columns (specific types of information) & rows (contains entries). Relationship between tables called `Schema`
    - For example: PostgreSQL, MySQL, Access, TERADATA, ORACLE DB, etc
 <p align="center"><img src="https://user-images.githubusercontent.com/64508435/92751029-33fe6c80-f3ba-11ea-9084-9c9470bc0eb3.png" alt="drawing" height="200"/></p>

   - Relational DB communicates with Back-End Server via `SQL` (SQL plays a role like HTTP did btw Front-End & Back-End)

2. **Non-Relational DB (NoSQL)**: allows you to build application without to define a clear Schema first.
   - `document-oriented` 
   - Non-Relational DB, such as MongoDB, communicates with Back-End Server via `MongoDB Query`

## 1.4. Life Cycle of a Database
- **1. Planning**: what is the pros and cons of building a DB? What are the specification of the data to be stored?
- **2a. Conceptual modeling/Analysis**: 
   - List of business rules (bullet points) in plain English that the organization requires
   - Produce ER diagram based on those rules
- **2b. Database design/Logical DB design**
   - Input: ER diagram
   - Output: Relational schema/logical diagram
- **3a. DB implementation**:
   - Input: Relational schema
   - Write SQL queries 
- **3b. DB maintenance**
   - Modify the DB to reflect thew new needs/change of business needs (management to decide with change in business needs, to start a new DB or change current DB)

### 1.4.1. Conceptual Database Design
- Business Rules
- E-R Model
- Entities
- Attributes
- Relationships
#### Business Rules
- e.g. build a DB for SIS to keep track of students, courses &#8594; Rules:
   - 1 course can have multiple students
   - Maximum of 50 students per class
- Guidlines for Business Rules:
   - **Declarative**: each session only max 50 students. Don’t care how
   - **Precise**: as clear as possible to avoid misunderstanding
   - **Atomic**: one statement per rule
   - **Consistent**: No 2 rules conflict each other, and not contradict itself
   - **Expressible**: Can be written and understandable in English
   - **Distinct**: no redundant rule
   - **Business Oriented**: Understood by business student


[(Back to top)](#table-of-contents)


# 2. SQL commands
<p align="center"><img src="https://user-images.githubusercontent.com/64508435/128194707-4511d049-bcdc-4823-925f-56d6509a18b6.png" alt="drawing" height="350"/></p>

- **DCL** (Data Control Language): `GRANT` or `REVOKE` access.
- **DDL** (Data Definition Language): to setup the data
- **DQL** (Data Query Language)
- **DML** (Data Modification Language): to modified the data in database
## 2.1. Basic SQL commands
- `' '` used for **Writing Text** 
- `" "` used for **Tables, Columns's Name**: `SELECT birth_date AS "DoB" FROM "public"."employees" WHERE first_name = 'Quan';`

| Function| Commands|
|---|---|
|Access SQL terminal| - Start PostgreSQL: `brew services start postgresql`<br> - Connect DB: `psql 'db_name'` |
|||
|Create Table| <br> `CREATE TABLE users (name text, age smallint, birthday date);`<br><br> List of PostgreSQL Data Types: https://www.postgresql.org/docs/9.5/datatype.html|
|List Table Relations| `\d`|
| Insert Into | `INSERT INTO users (name, age, birthday) VALUES ('Quan Nguyen', 25, '1995-09-10');` |
|||
| Select Data| `SELECT name, age, birthday FROM users;` <br>`SELECT * FROM users;`|
||Select Data with column 'name' starts with Q<br> `SELECT * FROM users WHERE name LIKE 'Q%';`|
||Select Data with column 'name' ends with g<br> `SELECT * FROM users WHERE name LIKE '%g';`|
||Order Data `DESC` & `ASC`<br>`SELECT * FROM users ORDER BY score DESC;`<br>|
| Rename Data | `SELECT column AS 'new_name'`|
| Concat Column|`SELECT CONCAT(first_name, ' ', last_name) as "Full Name" FROM employees;` <br>**Note**: use `''` in `CONCAT()` Statement as `""` is used in other SQL statement |
|||
|Aggregate Functions| - Calculate **AVG()**<br>`SELECT AVG(score) FROM users;` <br> - Calculate **SUM()** <br> - **COUNT()**, **MIN()**, **MAX()**<br>- Practise: [Exercise](./questions/0_Agrregate_Functions.sql)|
|||
|Comment| - Inline Comment `--` <br>- Block Comment `/* */` |
|||
| Alter Table | Add column, for ex: `score` column, into Table<br> `ALTER TABLE users ADD score smallint;`|
| Update Data | `UPDATE users SET score = 50 WHERE name='Thuy Dung' OR name='Quan';`|
| Delete Data |`DELETE FROM users WHERE name='Thuy Dung';`|
| Delete Table | `DROP TABLE login;`|
| Delete DB | `DROP DATABASE db_name;`|
## 2.2. Filtering Data
- `WHERE` Clause must **AFTER** `FROM`
- Logic Operators: `AND`, `OR`, `NOT`
## 2.3. Joining Tables

```
CREATE TABLE login (
    ID serial NOT NULL PRIMARY KEY,
    secret VARCHAR(100) NOT NULL,
    name text UNIQUE NOT NULL
);
```
- ID: 
   - `serial` allows to auto increment the ID
   - `PRIMARY KEY` generates a `login_id_seq` 
   ```
   test_db=# \d
                  List of relations
    Schema |     Name     |   Type   |   Owner    
   --------+--------------+----------+------------
    public | login        | table    | quannguyen
    public | login_id_seq | sequence | quannguyen
   ```

#### Joining SQL commands

` SELECT * FROM users JOIN login ON users.name = login.name;`
```
   name    | age |  birthday  | score | id | secret |   name    
-----------+-----+------------+-------+----+--------+-----------
 Thuy Dung |  24 | 1996-09-22 |    50 |  2 | xyz    | Thuy Dung
```

##### LEFT JOIN
- `LEFT JOIN` returns all records from the left table (table1), and the matched records from the right table (table2). The result is NULL from the right side, if there is no match.
```
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name = table2.column_name;
```
[(Back to top)](#table-of-contents)

# 3. Frequently-used SQL commands
| Function          | Description |Commands|
|-------------------|-------------|--------|
| `DISTINCT`        | Exclude duplicates from the answer| `SELECT DISTINCT(City) FROM Station;`|
| Odd & Even Number | `%, =, <>`| `SELECT City FROM Station WHERE Id % 2 <> 0;` <br>Select rows where ID are odd numbers|
| `COUNT`           | Count the number of record| `SELECT COUNT(City) - COUNT(DISTINCT(City)) FROM Station;`<br>Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.|
|`LENGTH`           | Count the word-length|`SELECT City, LENGTH(City) FROM Station`|
|`TOP`              | Select first few items, similar to df.head() in pandas| `SELECT TOP number or percent column_name(s);`<br> Select the first three records from the "Customers" <br>`SELECT TOP 3 * FROM Customers;`<br>Select the first 50% of the records from the "Customers"<br>`SELECT TOP 50 PERCENT * FROM Customers;`|
|`LIMIT`            | Limit the record, similar to TOP, but need to `ORDER BY`| `SELECT City, LENGTH(City) AS Length_City FROM Station ORDER BY Length_City ASC, City ASC LIMIT 1;`<br> If there is more than one smallest length of city name, choose the one that comes first when ordered alphabetically.|

[(Back to top)](#table-of-contents)

# 4. SQL Function
- A template of an SQL Function
```sql
CREATE FUNCTION functionName(N INT) RETURNS INT
BEGIN
<--Body-->
RETURN (
<--Select SQL Statement-->
);
END
```

# Cartesian product
- A Cartesian product joins every record in the first table with every record in the second table, 
- For example, ince the table has 4 rows and it's joined with itself => the output will be 4*4 = 16 records

```SQL
SELECT *
FROM Employee AS a, Employee AS b;
```
- Before : the default `Employee` Table
```
+----+-------+--------+-----------+
| Id | Name  | Salary | ManagerId |
+----+-------+--------+-----------+
| 1  | Joe   | 70000  | 3         |
| 2  | Henry | 80000  | 4         |
| 3  | Sam   | 60000  | NULL      |
| 4  | Max   | 90000  | NULL      |
+----+-------+--------+-----------+
```

- After : Cartesian product 
<img width="648" alt="Screenshot 2021-01-29 at 8 32 13 AM" src="https://user-images.githubusercontent.com/64508435/106215750-942c6880-620c-11eb-8d40-b8c7715bccbb.png">


# LeetCode
### 175. Combine Two Tables
Write a SQL query for a report that provides the following information for each person in the `Person` table, regardless if there is an address for each of those people: (FirstName, LastName) from Person table and  (City, State) from Address table.
- Learn: `LEFT JOIN`

```SQL
SELECT Person.FirstName, Person.LastName, Address.City , Address.State
FROM Person LEFT JOIN Address ON Person.PersonID = Address.PersonID;
```

### 176. Second Highest Salary
Write a SQL query to get the `second highest salary` from the Employee table. If there is no second highest salary, then the query should return `null`.
- Learn: `IFNULL((SELECT expression), null)`, `DISTINCT`, `LIMIT with OFFSET`, Header return using `AS`
```SQL
SELECT
IFNULL ((SELECT DISTINCT Salary
FROM Employee
ORDER BY Salary Desc
LIMIT 1 OFFSET 1),NULL) AS SecondHighestSalary;
```
### 177. Nth Highest Salary
Write a SQL query to get the nth highest salary from the Employee table.
- Learn: `LIMIT 1 OFFSET M = LIMIT M, 1`, DECLARE M INT;
```SQL
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
DECLARE M INT;
SET M=N-1;
RETURN (
    # Write your MySQL query statement below.
    SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT M, 1);
END
```
### 181. Employees Earning More Than Their Managers
Given the Employee table, write a SQL query that finds out employees who earn more than their managers.
- Learn: [Cartesian Product](#cartesian-product)
- Solution 1: Cartesian Product
```SQL
SELECT a.Name AS `Employee`
FROM Employee AS a, Employee AS b
WHERE a.ManagerId = b.Id AND a.Salary > b.Salary;
```
- Solution 2: LEFT JOIN
```SQL
SELECT a.Name as 'Employee'
FROM Employee AS a LEFT JOIN Employee AS b 
ON a.ManagerId = b.Id 
WHERE a.Salary > b.Salary;
```

### 182. Duplicate Emails

Write a SQL query to find all duplicate emails in a table named Person.
- Learn: 
   - `HAVING` clause was added to SQL because the `WHERE` keyword could not be used with aggregate functions like `COUNT`
   - to add a condition to a `GROUP BY` is to use the `HAVING` clause
   - Order: `WHERE` > `GROUP BY` >  `HAVING` 

```SQL
# Write your MySQL query statement below
SELECT Email
FROM Person
GROUP BY Email
HAVING COUNT(Email) > 1;
```
### 183. Customers Who Never Order
Suppose that a website contains two tables, the Customers table and the Orders table. Write a SQL query to find all customers who never order anything.
- Learn:
   - `IS NULL` : to check if any entry is NULL
```SQL
SELECT c.Name AS 'Customers'
FROM Customers AS c LEFT JOIN Orders AS o 
ON c.Id = o.CustomerId  
WHERE o.CustomerId IS NULL;
```

### 196. Delete Duplicate Emails
Write a SQL query to delete all duplicate email entries in a table named Person, keeping only unique emails based on its smallest Id.
- Learn #1: How to Delete an Item in SQL 
- Learn #2: `SELECT FROM ( SELECT FROM )`

```SQL
# Write your MySQL query statement below
DELETE FROM PERSON WHERE Id NOT IN (
    SELECT Id FROM (
        SELECT MIN(id) as Id 
        FROM Person
        GROUP BY Email
    ) t
);


```

# Appendix
## A1. Relational Database PostgreSQL
## Database Creation & Connectivity
|Step |Description   |   
|---|---|
| Install PostgreSQL GUI  | Link: https://www.postgresql.org/download/  |  
|Install Homebrew (if not installed)| *HomeBrew*: cài thứ bạn cần mà Apple không cung cấp<br>- To install, Paste this into Terminal: <br> `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`<br>-Update: `brew update`<br>-If some file missing: `brew doctor`|
| Install PostgreSQL | `brew install postgresql` |
|||
| Start PostgreSQL | `brew services start postgresql` |
| Create a new DB| `createdb 'db_name'` |
| Connect DB | `psql 'db_name'` |
| Connect DB via PostgreSQL GUI| You also can connect to PostgreSQL via GUI by providing `db_name` <img src="https://user-images.githubusercontent.com/64508435/93224480-b795d000-f7a3-11ea-9cf6-38034c225ecb.png" width="500"> |
| Stop PostgreSQL | `brew services stop postgresql` |

[(Back to top)](#table-of-contents)

