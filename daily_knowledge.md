# SQL Daily Knowledge
## Day 4
### Best Practices
- **SQL identifiers**: use double quotes `"` around `table and column names`
- SQL also has strings and we use single quotes `'` around `strings` to differentiate them from identifiers.
## Day 3
- To view how many databases avail
```sql
-- To show all the db avail
SHOW DATABASES
-- Use a certain db
USE db_name
-- show all tables inside "db_name" db
SHOW TABLES 
SELECT * FROM table1;


```
- To view table schema/details
```sql
SHOW CREATE TABLE table_name
SHOW CREATE VIEW view_name
```
- `WITH temp_table AS` [Common Table Expression (CTE)](https://learnsql.com/blog/what-is-common-table-expression/)
```sql
WITH my_cte AS (
  SELECT a,b,c
  FROM T1
)
SELECT a,c
FROM my_cte
WHERE ....

```
### SQL Optimisation
#### Join 
- Join the smaller table in the second part of the join
- Filter tables before joining
- Use **partition** column first in the join condition (if possible) before other conditions
## Day 2
### Find all duplicate records in a table
- For example: table `USERS`
    - record is considered duplicate if a user name is present more than once
```sql
-- USERS table
-- In this case, Robin (id=4 and 5) are duplicate record)
create table users
(
user_id int primary key,
user_name varchar(30) not null,
email varchar(50));

insert into users values
(1, 'Sumit', 'sumit@gmail.com'),
(2, 'Reshma', 'reshma@gmail.com'),
(3, 'Farhana', 'farhana@gmail.com'),
(4, 'Robin', 'robin@gmail.com'), 
(5, 'Robin', 'robin@gmail.com');
```
- Expect Output: 
<img src="https://github.com/CodexploreRepo/sql/assets/64508435/719415f6-7bde-459f-ba1f-7804b92c19a8" width=500 >

- Solution:
    - Step 1: Using `Window Function` to partition the data based on `user_name` and then give a row number to each of the partitioned user name. If a user name exists more than once then it would have multiple row numbers.
        - ```sql
            select *,
                row_number() over (partition by user_name order by user_id) as rn
            from users 
            order by user_id
          ```
        - ![Screenshot 2023-06-22 at 22 23 25](https://github.com/CodexploreRepo/sql/assets/64508435/c42bff02-8f0b-484e-82d6-673f81e8d918)
    
    - Step 2: Using the row number which is other than 1, we can identify the duplicate records. In this case is **Robin**.

```sql
-- final solution
select user_id, user_name, email
from (
    select *,
        row_number() over (partition by user_name order by user_id) as rn
    from users 
    order by user_id
    ) x
where x.rn > 1;
```
## Day 1
### SQL Stacking
```sql
--original df
/*
+---+-----+-----+-----+
|id |team1|team2|team3|
+---+-----+-----+-----+
|1  |30   |300  |3000 |
|2  |50   |500  |5000 |
|3  |100  |1000 |10000|
|4  |200  |2000 |20000|
+---+-----+-----+-----+
*/
-- where 3 is total number of cols to be stacked into single row, in this case is 3 (team1, team2, team3)
SELECT id, STACK(3, 'team1_new', team1, 'team2_new', team2, 'team3_new', team3) AS (team, points) FROM df
-- Result
/*
+---+---------+------+
| id|     team|points|
+---+---------+------+
|  1|team1_new|    30|
|  1|team2_new|   300|
|  1|team3_new|  3000|
|  2|team1_new|    50|
......................
+---+---------+------+
*/

```
### Type Casting
- `CAST col AS data_type` casting column Data Type
    - Example: `where a.category_id = CAST(b.id AS INT)` as id from table b does not have the same data type INT as category_id of table a
### SQL Tricks
```sql
/* Math */
-- Even ID number     => (ID%2)=0

/* SQL Functions */
-- Exclude duplicates => DISTINCT()
-- Count Entries      => COUNT()
-- Count word-length of each entry => LENGTH()
-- Limit the entries  => LIMIT 10
/* Regular Expression */ 
-- REGEX: start with ^ and end with $
-- Match 'aeoui' as the beginning character => RLIKE "^[aeiou].*$"

SELECT DISTINCT(CITY) FROM STATION WHERE (ID%2)=0 ;

-- Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries
SELECT (COUNT(CITY) - COUNT(DISTINCT(CITY))) FROM STATION;

-- Query the two cities in STATION with the longest CITY names, as well as their respective lengths 
SELECT CITY, LENGTH(CITY) 
FROM STATION 
ORDER BY 
    LENGTH(CITY) DESC, 
    CITY ASC
LIMIT 1;

-- Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION
SELECT DISTINCT CITY FROM STATION WHERE CITY RLIKE "^[aeiou].*$"
```
