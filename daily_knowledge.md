# SQL Daily Knowledge
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
