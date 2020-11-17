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
