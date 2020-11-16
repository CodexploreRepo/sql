```sql
-- Exclude duplicates => DISTINCT()
-- Even ID number     => (ID%2)=0
SELECT DISTINCT(CITY) FROM STATION WHERE (ID%2)=0 ;


```
