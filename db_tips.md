```sql
/* Math */
-- Even ID number     => (ID%2)=0

/* SQL Functions */
-- Exclude duplicates => DISTINCT()
-- Count Entries      => COUNT()
-- Count word-length of each entry => LENGTH()



SELECT DISTINCT(CITY) FROM STATION WHERE (ID%2)=0 ;

-- Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries
SELECT (COUNT(CITY) - COUNT(DISTINCT(CITY))) FROM STATION;

-- Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths 

```
