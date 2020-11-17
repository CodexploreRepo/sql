```sql
-- Exclude duplicates => DISTINCT()
-- Even ID number     => (ID%2)=0
SELECT DISTINCT(CITY) FROM STATION WHERE (ID%2)=0 ;

-- Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries
SELECT (COUNT(CITY) - COUNT(DISTINCT(CITY))) FROM STATION;

```
