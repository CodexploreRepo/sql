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
### RANGE
- Use the operators `<`, `>`, `<=` and `>=` in our conditions to match a range of values.
```sql
SELECT "title", "author" 
FROM "longlist" 
WHERE "year" >= 2019 AND "year" <= 2022;
```
- Use keywords `BETWEEN` and `AND` to specify inclusive ranges
```sql
SELECT "title", "author" 
FROM "longlist" 
WHERE "year" BETWEEN 2019 AND 2022;
```
### `NULL`
- It is possible that tables may have missing data. `NULL` is a type used to indicate that certain data does not have a value, or does not exist in the table.
- Conditions used with NULL are `IS NULL` and `IS NOT NULL`.

### `LIKE`
- `LIKE` is used to select data that roughly matches the specified string
  -  `%` matches 0 or more characters around a given string
  -  `_` matches a single character
-  Example 1: `%`
```sql
-- To select the books with the word “love” in their titles, we can run
SELECT "title"
FROM "longlist"
WHERE "title" LIKE '%love%';

-- To select the books whose titles begin with the word “The”
SELECT "title" 
FROM "longlist" 
WHERE "title" LIKE 'The %';
```
- Example 2: `_`
```sql
-- To select a book in the table whose name is either “Pyre” or “Pire”
SELECT "title" 
FROM "longlist" 
WHERE "title" LIKE 'P_re';

-- a book in the table whose title begins with “T” and has four letters
SELECT "title" 
FROM "longlist" 
WHERE "title" LIKE 'T____';
```

## `ORDER BY`
- The `ORDER BY` keyword allows us to organize the returned rows in some specified order
```sql
-- can order by multiple cols
SELECT "title", "rating", "votes" 
FROM "longlist"
ORDER BY "rating" DESC, "votes" ASC
LIMIT 10;
```

## Aggregate Functions
- `COUNT`, `AVG`, `MIN`, `MAX`, and `SUM` are called aggregate functions and allow us to perform the corresponding operations over multiple rows of data
- Use `ROUND` to round the result
- Use `DISTINCT` to only count distinct values
```sql
-- use * to select every row and column from the database
SELECT COUNT(*) 
FROM "longlist";
-- use ROUND
SELECT ROUND(AVG("rating"), 2) AS "average rating" 
FROM "longlist";
-- use DISTINCT
SELECT COUNT(DISTINCT "publisher") 
FROM "longlist";
```
