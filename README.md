# Database
ISSS615 - Data Management
# Table of contents
- [Table of contents](#table-of-contents)
- [1. Introduction to Database](#1-introduction-to-database)
   - [1.1. Important Concepts](#11-important-concepts)
   - [1.2. Database Management System](#12-database-management-system)
   - [1.3 Types of DB](#13-types-of-db)
   - [1.4. Life Cycle of a Database](#14-life-cycle-of-a-database)
   - [1.5. ER Model](#15-er-model)
      - [1.5.1. Entity](#151-entity)
      - [1.5.2. Attributes](#152-attributes)
      - [1.5.3. Relationships](#153-relationships)
   - [1.6. Enhanced ER](#16-enhanced-er)
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

## 1.5. ER Model
- **Entity-Relationship Model (E-R Model)**: a logical representation of the data for an organization or for a business area
- **Entity-Relationship Diagram (E-R Diagram)**: a graphical representation of an entity-relationship model
   - `Entity (Entity Type)` is a Rectangle box (collection of objects that share common properties or characteristics)
   - `Attributes` are written inside each box
   - `Relationships` are drawn as lines that connect entities
<p align="center"><img width="523" alt="Screenshot 2021-12-14 at 22 11 13" src="https://user-images.githubusercontent.com/64508435/146024992-0777673e-345f-44ac-9e30-e03e4ce7bb43.png"></p>

### 1.5.1. Entity
- **Entity (Entity Type)**: A person, place, object, event, or concept in the user environment about which the organization wishes to maintain data
- Entity name is in *singular form*: Student, not Students
### 1.5.2. Attributes
- **Attribute**: property or characteristic of an entity type
- Classification of attributes:
   - Simple vs. **Composite Attribute** (made up of multiple component attributes, denoted as: `composite attribute (comp attr 1, comp attr 2, etc)`)
   - Single-Valued vs. **Multi-valued Attribute** (denoted as: `{multi-valued attribute}`)
   - Stored vs. **Derived Attributes** (attributes that can be derived from other attributes & *Derived attributes will not be stored*, denoted as: `[derived_attributes]`)
      - Assumption: To standardize the requirement, all the derived attributes must be included into ER models
<p align="center"><img width="523" alt="Screenshot 2021-12-16 at 21 55 31" src="https://user-images.githubusercontent.com/64508435/146394735-684e6aa7-0098-407b-8fdb-705c53ada6ae.png"></p>

- `Employee_Address` is composite attribute, made up of component attributes: street address, city, state, postal code.
<p align="center"><img width="620" alt="Screenshot 2021-12-16 at 22 07 50" src="https://user-images.githubusercontent.com/64508435/146396695-829b0a36-1d41-49f8-89eb-e4f77fb961cd.png"></p>

- Derived attributes are attributes that can be derived from other attributes, in this case is from date employed and current date, denoted as: `[years_employed]`

   - **Identifier Attributes**: *uniquely* identifies individual instances of an entity type,  denoted as: **Underline**
      - Simple Identifier vs. Composite Identifier (For example: Flight_ID (Flight number, Date))
      - For example: in Student entity, Name is not a good identifier candidate as many students can have the same name, Student ID is a possible identifier as it is unique. 

<p align="center"><img width="573" alt="Screenshot 2021-12-16 at 22 18 22" src="https://user-images.githubusercontent.com/64508435/146398353-d0855de8-082d-4c12-a7b7-1ae1614a90bc.png"></p>

### 1.5.3. Relationships
- Relationships can have attributes
- Two entities can have more than one type of relationship between them (multiple relationships)
- **Relationship with Attributes**: When the attributes depend on all the entities that participate in the relationship, the attributes belong to the relationship
   - For example: Each employee has a common `Complete_Date` and a common `Grade` for all the courses he/she completes
   <p align="center"><img width="527" alt="Screenshot 2021-12-21 at 16 45 11" src="https://user-images.githubusercontent.com/64508435/146908309-50ccc82b-ae69-4b55-8d98-87d31b509c35.png"></p>

- **Degree of a relationship**: Number of entities participating in the relationship
   - Unary Relationship
   - Binary Relationship
   - Ternary Relationship, or higher
   <p align="center"><img width="621" alt="Screenshot 2021-12-16 at 22 32 47" src="https://user-images.githubusercontent.com/64508435/146400768-c281a1fb-f8e2-4884-815e-b9ad8e1a03b6.png"></p>

- **Cardinality of relationship**: Number of instances of one entity that can or must be associated with each instance of another entity
   - `One – to – One`: Each entity in the relationship will have exactly one related entity
   - `One – to – Many`: An entity on one side of the relationship can have many related entities, but an entity on the other side will have a maximum of one related entity
   - `Many – to – Many`: Entities on both sides of the relationship can have many related entities on the other side
<p align="center"><img width="577" alt="Screenshot 2021-12-16 at 22 36 28" src="https://user-images.githubusercontent.com/64508435/146401360-37d64d1c-3c77-48a1-99d3-34199180dff7.png"></p>

   - **Cardinality Constraints** - the number of instances of one entity that can or must be associated with each instance of another entity
<p align="center"><img width="629" alt="Screenshot 2021-12-16 at 22 43 04" src="https://user-images.githubusercontent.com/64508435/146402541-1e95e6dc-561a-4355-9067-4477c2a35d18.png"></p>


[(Back to top)](#table-of-contents)

## 1.6. Enhanced ER
### 1.6.1. Supertype and Subtype
- **Supertype**: A generic entity type that has a relationship with one or more subtypes
- **Subtype**: A subgrouping of the entities in an entity type which has attributes that are distinct from those in other subgroupings
- *Inheritance*:
   - Subtype entities inherit values of all attributes of the supertype
   - An instance of a subtype is also an instance of the supertype
- For example: Car, Truck, and Motor Cycle share the common attributes. So, we *generalize* the shared attributes in a **supertype**. Unique attributes are still with **subtypes**.
   - All instances of car will inherit all attributes & identifier of vehicle + special attribute No_of_passengers
   - Subtypes: Car & Truck
   - Each circle is connected to exactly one Supertype and one or many subtypes
   - Note: **2 subtypes (motorbike & bike) with no special attribute, we DO NOT draw any of them.**
<p align="center"><img width="500" alt="Screenshot 2021-12-21 at 17 18 51" src="https://user-images.githubusercontent.com/64508435/146913315-5a37087f-8b4a-4627-a88e-d7af7bdf1eac.png">
<img width="300" alt="Screenshot 2021-12-21 at 17 42 35" src="https://user-images.githubusercontent.com/64508435/146916808-21a2ff86-3cf3-471f-8853-facaf60ced45.png"></p>

#### Relationship & Supertypes
- Relationships at the supertype level indicate that all subtypes will participate in the relationship
<p align="center"><img width="500" alt="Screenshot 2021-12-21 at 17 57 53" src="https://user-images.githubusercontent.com/64508435/146919001-489af95a-816d-4e94-9168-d9e757bb26ca.png"></p>


### 1.6.2. Generalization and Specialization
- **Generalization**: The process of defining a more general entity type from a set of more specialized entity types- `BOTTOM-UP`
- **Specialization**: The process of defining one or more subtypes of the supertype, and forming supertype/subtype relationships- `TOP-DOWN`

### 1.6.3.Constraints in Supertype
#### 1.6.3.1. Completeness Constraints
- Whether an instance of a supertype must also be a member of at least one subtype
   - `Total Specialization Rule` (Double line): Supertype cannot have its own instances 
   - `Partial Specialization Rule` (Single line): Supertype can have own instances 
<p align="center">
<img width="500" alt="Screenshot 2021-12-21 at 22 46 17" src="https://user-images.githubusercontent.com/64508435/146958480-abbe6dfd-967a-46a1-b868-860b3898259c.png"><br>Example: Total Specialization Rule
</p>

<p align="center">
<img width="500" alt="Screenshot 2021-12-21 at 22 46 17" src="https://user-images.githubusercontent.com/64508435/146958915-66296824-55c3-430e-9914-8191644bba5f.png"><br>Example: Partial Specialization Rule
</p>

#### 1.6.3.2. Disjointness Constraints
- Whether an instance of a supertype may simultaneously be a member of two (or more) subtypes
   - `Disjoint Rule`: An instance of the supertype can be only ONE of the subtypes (represented by “`d`”)
   - `Overlap Rule`: An instance of the supertype could be more than one of the subtypes (represented by “`o`”)

<p align="center">
<img width="500" alt="Screenshot 2021-12-21 at 22 46 17" src="https://user-images.githubusercontent.com/64508435/146959675-348ead81-46d3-455b-8bb2-452d5d23ce50.png"><br>Example: Disjoint Rule
</p>
<p align="center">
<img width="500" alt="Screenshot 2021-12-21 at 22 46 17" src="https://user-images.githubusercontent.com/64508435/146959941-611399f5-8c08-43c0-86dd-f5561d696754.png"><br>Example: Overlap Rule
</p>

#### 1.6.3.3. Subtype Discriminators
- An attribute of the supertype whose values determine the target subtype(s)
   - Disjoint – a simple attribute with alternative values to indicate the possible subtypes
   - Overlapping – a composite attribute whose subparts pertain to different subtypes. Each subpart contains a boolean value to indicate whether or not the instance belongs to the associated subtype
- Requirement: **All the ER models with supertype/subtype must have** `Subtype Discriminator`
<p align="center">
<img width="500" alt="Screenshot 2021-12-21 at 22 46 17" src="https://user-images.githubusercontent.com/64508435/146961055-b4ca5122-52b2-4b1e-b810-72ecf2c87c02.png"><br>Example: Introducing a subtype discriminator (disjoint rule)
</p>
<p align="center">
<img width="500" alt="Screenshot 2021-12-21 at 22 46 17" src="https://user-images.githubusercontent.com/64508435/146961129-78ce6ddb-1c3e-414d-beb0-bec363c6fc93.png"><br>Example: Subtype discriminator (overlap rule)
</p>

- If overlap, then the discriminator will be a composite of Boolean value
- If total specialisation, Type will be (Y,Y) (Y,N), (N,Y), cannot have (N,N)


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
|Sort|Order Data `DESC` & `ASC`<br>`SELECT * FROM users ORDER BY score DESC;`<br>|
| Rename Data | `SELECT column AS 'new_name'`|
| Concat Column|`SELECT CONCAT(first_name, ' ', last_name) as "Full Name" FROM employees;` <br>**Note**: use `''` in `CONCAT()` Statement as `""` is used in other SQL statement |
|||
|Aggregate Functions| - Calculate **AVG()**<br>`SELECT AVG(score) FROM users;` <br> - Calculate **SUM()** <br> - **COUNT()**, **MIN()**, **MAX()**<br>- Practise: [Exercise](./questions/0_Agrregate_Functions.sql)|
|Round Up/Down|`FLOOR(), CEILING()`|
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

### 2.2.1 CASE STATEMENT
- The `CASE` statement like an if-then-else statement.
```Python
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
```
- Write a query identifying the type of each record in the TRIANGLES table using its three side lengths "Isosceles, Equilateral, Scalene".
```SQL
SELECT 
    CASE
        WHEN A+B > C AND A+C > B AND B+C > A THEN
        CASE
            WHEN A = B AND A = C THEN "Equilateral"
            WHEN A = B OR B = C OR A = C THEN "Isosceles"
            ELSE "Scalene"
        END
        ELSE "Not A Triangle"
    END
FROM TRIANGLES;

```

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
|`REPLACE`| To replace certain char in a column| `SELECT CEILING(AVG(SALARY) - AVG(REPLACE(SALARY, 0 ,''))) FROM EMPLOYEES;` <br> To replace 0 with '' in a Salary column |


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

## MySQL
Link: https://flaviocopes.com/mysql-how-to-install/
```Python
mysql.server start
mysql -u root -p #This is to connect to mySQL server via Root user

mysql.server stop


```

[(Back to top)](#table-of-contents)

