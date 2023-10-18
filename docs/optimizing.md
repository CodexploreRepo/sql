# Optimizing
## Introduction
- For example: the Internet Movies Database, or IMDb
<p align="center"><img width="92" alt="Screenshot 2023-10-13 at 5 18 52 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/f474d4da-c34b-4d20-9804-2129a71b8c22">
<img width="500" alt="Screenshot 2023-10-13 at 5 28 22 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/3fc4c713-8134-49f0-b8a7-952a3ff7f219">
</p>

-  To find the information pertaining to the movie Cars, we would run the following query.
  - The time taken to execute this query during lecture was roughly a tenth of a second as the IMDb is super large.
  - Under the hood, when the query to find Cars was run, we triggered a scan of the table movies — that is, the table movies was scanned top to bottom, one row at a time, to find all the rows with the title Cars.
```sql
SELECT * FROM "movies"
WHERE "title" = 'Cars';
```
- How to optimize this query to be more efficient than a scan. I
## Index
- An index, in database terminology, is a structure used to speed up the retrieval of rows from a table.
- In most of database management systems, if we specify that a column is a **primary key**, an index will automatically be created via which we can search for the primary key. However, for regular columns like "title", there would be no automatic optimization.
- We can use the following command to create an index for the "title" column in the movies table.
  - It will take sometimes to create the index 
```sql
CREATE INDEX "title_index" ON "movies" ("title");  

-- After creating this index, we run the query to find the movie titled Cars again. On this run, the time taken is significantly shorter
SELECT * FROM "movies"
WHERE "title" = 'Cars';
```
- To remove the index we just created, run
```sql
DROP INDEX "title_index";
```
- Running `EXPLAIN QUERY PLAN` again with the `SELECT` query will demonstrate that the plan
  - As you can see in the screenshot below, the query plan will be "SEARCH" through `movies` table using INDEX `title_index` instead of "SCAN"
```sql
EXPLAIN QUERY PLAN
SELECT * FROM "movies" WHERE "title" = 'Cars';
```
<p align="center">
<img width="450" alt="Screenshot 2023-10-13 at 5 28 22 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/f937cb20-2d63-4106-84eb-a1b3b6c1d5ec">
</p>

### How to find Index Column
- To understand what kind of index could help speed this query up, we can run `EXPLAIN QUERY PLAN` ahead of the query.
- For example:
  - In this query, it starts "SCAN" `people` table in SUBQUERY 1 &#8594; need to create the index for `name` column in `people` table
  - Then it "SCAN"s in `stars` table and return as a list &#8594; need to create  the index for `person_id` column in `stars` table
  - Before it can "SEARCH" in movies using `rowid` which are the primary key &#8594; no need as the operation is already "SEARCH"
```sql
EXPLAIN QUERY PLAN
SELECT "title" FROM "movies"
WHERE "id" IN (
    SELECT "movie_id" FROM "stars"
    WHERE "person_id" = (
        SELECT "id" FROM "people"
        WHERE "name" = 'Tom Hanks'
    )
);
```
<p align="center">
<img width="450" alt="Screenshot 2023-10-13 at 5 28 22 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/2321113e-b845-42f2-a6fb-468fd5a16d3f">
</p>

- Let us create the two indexes to speed this query up.
```sql
CREATE INDEX "person_index" ON "stars" ("person_id");
CREATE INDEX "name_index" ON "people" ("name");
```
- Now, we run `EXPLAIN QUERY PLAN` with the same nested query. We can observe that
  - All the scans are now searches using indexes, which is great!
  - The search on the table `people` uses something called a `COVERING INDEX`
<p align="center">
<img width="450" alt="Screenshot 2023-10-13 at 5 28 22 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/4f4c8b76-5603-4da0-9122-7299f8d500a8">
</p>

### Covering Index
- A covering index means that all the information needed for the query can be found within the index itself.
- From above example, we create `name` as the index in the `people`, so when we perform the filtering use `WHERE "name" = 'Tom Hanks'`
  - we are looking up relevant information directly in the index instead of (looking up relevant information in the index &#8594; using the index to then search the table)


