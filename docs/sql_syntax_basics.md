# SQL Syntax - Basics
## `SELECT`
- What data is actually in our database? To answer this, we will use our first SQL keyword, `SELECT`, which allows us to select some (or all) rows from a table inside the database.
```sql
SELECT * 
FROM "longlist" 
```
## `LIMIT`
- If a database had millions of rows, it might not make sense to select all of its rows. Instead, we might want to merely take a peek at the data it contains. We use the SQL keyword `LIMIT` to specify the number of rows in the query output.
```sql
SELECT "title" 
FROM "longlist" 
LIMIT 10;
```
## `WHERE`
- The keyword `WHERE` is used to select rows based on a condition
- Conditions in SQL:  `=` (“equal to”), `!=` (“not equal to”) and `<>` (also “not equal to”)
  - Another way to get the "not equal" results is to use the SQL keyword`NOT`
  ```sql
  SELECT "title", "format" 
  FROM "longlist" 
  WHERE NOT "format" = 'hardcover'; --NOT ("format" = 'hardcover') is equal to "format" <> 'hardcover'
  ``` 
- Combine conditions: we can use the SQL keywords `AND` and `OR`
```sql
SELECT "title", "format" 
FROM "longlist" 
WHERE ("year" = 2022 OR "year" = 2023) AND "format" != 'hardcover';
```
