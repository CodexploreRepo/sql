# SQL Syntax - Creation & Insertion
## `CREATE TABLE`
### Data Types and Storage Classes
- A storage class can hold several data types.
  - For example, these are the data types that fall under the umbrella of the `Integer` storage class.
<p align="center">
<img width="344" alt="Screenshot 2023-10-10 at 10 28 06 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/560b292a-797a-4548-a6aa-0a860f098b86"></p>

- SQLite has five storage classes:
  - `Null`: nothing, or empty value
  - `Integer`: numbers without decimal points
  - `Real`: decimal or floating point numbers
  - `Text`: characters or strings
  - `Blob`: Binary Large Object, for storing objects in binary (useful for images, audio etc.)

#### Type Affinities
- `Type affinities` meaning that they try to convert an input value into the type they have an affinity for
  - For example: Consider a column with a type affinity for Integers. If we try to insert “25” (the number 25 but stored as text) into this column, it will be converted into an integer data type.

### Table Constraints
- `PRIMARY KEY` column must have unique values.
- `FOREIGN KEY` a constraint on a foreign key value is that it must be found in the primary key column of the related table
<p align="center">
<img width="125" alt="Screenshot 2023-10-10 at 10 33 47 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/421af2e2-9a10-4d99-b7c6-e7f139953bcc">
</p>

```sql
CREATE TABLE riders (
    "id" INTEGER,
    "name" TEXT,
    PRIMARY KEY("id")
);

CREATE TABLE stations (
    "id" INTEGER,
    "name" TEXT,
    "line" TEXT,
    PRIMARY KEY("id")
);

CREATE TABLE visits (
    "id" INTEGER,
    "rider_id" INTEGER,
    "station_id" INTEGER,
    FOREIGN KEY("rider_id") REFERENCES "riders"("id"),
    FOREIGN KEY("station_id") REFERENCES "stations"("id")
);

```

### Column Constraints
- A column constraint is a type of constraint that applies to a specified column in the table.
  - `CHECK`: allows checking for a condition, like all values in the column must be greater than 0
  - `DEFAULT`: uses a default value if none is supplied for a row
  - `NOT NULL`: dictates that a null or empty value cannot be inserted into the column
  - `UNIQUE`: dictates that every value in this column must be unique
- Example of Column Constraints:
  - `DEFAULT CURRENT_TIMESTAMP` — it returns the year, month, day, hour, minute and second combined into one value
  - `CHECK` on "type" to ensure its value is one of ‘enter’, ‘exit’ and ‘deposit’. 
```sql
CREATE TABLE "swipes" (
    "id" INTEGER,
    "card_id" INTEGER,
    "station_id" INTEGER,
    "type" TEXT NOT NULL CHECK("type" IN ('enter', 'exit', 'deposit')),
    "datetime" NUMERIC NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "amount" NUMERIC NOT NULL CHECK("amount" != 0),
);
```

## `DROP TABLE`
- to drop (or delete) the existing tables: `DROP TABLE "table_name";`

## Altering Tables
- Any changes on the Table will start with the keyword `ALTER TABLE "table_name"`
- Rename table
```sql
-- rename "visits" to "swipes"
ALTER TABLE "visits"
RENAME TO "swipes";
```
- Add columns
```sql
-- add column "swipetype" TEXT to "swipes" table
ALTER TABLE "swipes"
ADD COLUMN "swipetype" TEXT;
```
- Rename column
```sql
ALTER TABLE "swipes"
RENAME COLUMN "swipetype" TO "type";
```
- drop (or remove) a column.
```sql
ALTER TABLE "swipes"
RENAME COLUMN "swipetype" TO "type";
```

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

## `GROUP BY` 
- Consider the `ratings` table.
- Example 1: For each book, we want to find the average rating of the book. To do this, we would first need to group ratings together by book and then average the ratings out for each book (each group).
```sql
SELECT "book_id", AVG("rating") AS "average rating"
FROM "ratings"
GROUP BY "book_id";
```
- Example 2: To find how many numbers of ratings each book has
```sql
SELECT "book_id", COUNT("rating")
FROM "ratings"
GROUP BY "book_id";
```
### `HAVING`
- `HAVING` keyword is used here to specify a condition for the groups
- instead of `WHERE` (which can only be used to specify conditions for individual rows).

```sql
SELECT "book_id", ROUND(AVG("rating"), 2) AS "average rating"
FROM "ratings"
GROUP BY "book_id"
HAVING "average rating" > 4.0
ORDER BY "average rating" DESC;
```

## `JOIN`
- `JOIN` keyword allows us to combine two or more tables together.
### INNER `JOIN`
- Only keep the matched data based on the conditions
```sql
SELECT *
FROM "sea_lions"
JOIN "migrations" ON "migrations"."id" = "sea_lions"."id";
```
### OUTER `JOIN`
- `OUTER JOIN` is a way of joining tables that allow us to retain certain unmatched IDs
- an `OUTER JOIN` could lead to empty or NULL values in the joined table.
#### `LEFT JOIN`
- `LEFT JOIN` prioritizes the data in the left (or first) table
```sql
SELECT *
FROM "sea_lions"
LEFT JOIN "migrations" ON "migrations"."id" = "sea_lions"."id";
```
<p align="center"><img width="550" alt="Screenshot 2023-10-09 at 5 05 29 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/d259e6c5-13d9-4916-ab32-1c5ef494fcc4"></p>

- This query would retain all sea lion data from the `sea_lions` table — the left one. Some rows in the joined table could be partially blank. This would happen if the right table didn’t have data for a particular ID.

#### `RIGHT JOIN`
- `RIGHT JOIN` retains all the rows from the right (or second) table. 
<p align="center"><img width="550" alt="Screenshot 2023-10-09 at 5 05 29 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/4ab88656-5928-442a-a5c0-ddb5bd1964f7"></p>

#### `FULL JOIN`
- `FULL JOIN` allows us to see the entirety of all tables (include the unmatched from both tables)
<p align="center">
<img width="550" alt="Screenshot 2023-10-09 at 5 09 52 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/ebb529a2-bb81-47a1-90be-21fa2e02183e">
</p>

## SET
### `INTERSECT`
- For example, in our database of books, we have authors and translators. A person could be either an author or a translator. If the two sets have an intersection, it is also a possible that a person could be both an author and a translator of books.
```sql
SELECT "name" FROM "translators"
INTERSECT
SELECT "name" FROM "authors";
```
<p align="center">
<img width="422" alt="Screenshot 2023-10-09 at 5 17 42 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/bbbf0b16-ed91-434c-b84e-1748faa31553">
</p>

- For example, we can find the books that Sophie Hughes and Margaret Jull Costa have translated together.
  - Each of the nested queries here finds the IDs of the books for one translator.
  - The `INTERSECT` keyword is used to intersect the resulting sets and give us the books they have collaborated on.
```sql
SELECT "book_id" FROM "translated"
WHERE "translator_id" = (
    SELECT "id" from "translators"
    WHERE "name" = 'Sophie Hughes'
)
INTERSECT
SELECT "book_id" FROM "translated"
WHERE "translator_id" = (
    SELECT "id" from "translators"
    WHERE "name" = 'Margaret Jull Costa'
);
```

### `UNION`
- If a person is either an author or a translator, or both, they belong to the union of the two sets. In other words, this set is formed by combining the author and translator sets.
  -  A minor adjustment to the previous query gives us the profession of the person in the result set, based on whether they are an author or a translator.

```sql
SELECT 'author' AS "profession", "name" 
FROM "authors"
UNION
SELECT 'translator' AS "profession", "name" 
FROM "translators";
```
<p align="center">
<img width="388" alt="Screenshot 2023-10-09 at 5 25 11 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/6f6375b3-5a03-410d-b243-60c6a5af97bc">
</p>

### `EXCEPT`
- Everyone who is an author and only an author is included in the following set.
- The `EXCEPT` keyword can be used to find such a set. In other words, the set of translators is subtracted from the set of authors to form this one.
```sql
SELECT "name" FROM "authors"
EXCEPT
SELECT "name" FROM "translators";
```
<p align="center">
<img width="388" alt="Screenshot 2023-10-09 at 5 28 05 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/5abb5748-1b7e-4f12-a576-0194446e1761">
</p>

### Special Case
- How can we find this set of people who are either authors or translators but not both?
<p align="center">
<img width="388" alt="Screenshot 2023-10-09 at 5 29 31 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/f545602f-ece0-44a3-8839-17a22e0d0f7f">
</p>

```sql
(SELECT "name" FROM "authors"
EXCEPT
SELECT "name" FROM "translators")
UNION
(SELECT "name" FROM "translators"
EXCEPT
SELECT "name" FROM "authors")
```
