# Views
## Introduction
- To find a book written by the author Han Kang, we would need to go each of through the three table above — first finding the author’s ID, then the corresponding book IDs and then the book titles. 
<p align="center"><img width="525" alt="Screenshot 2023-10-07 at 11 21 05 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/e46176d6-f43b-4557-bf12-867771d86c16"></p>

- We can use the JOIN command in SQL to combine rows from two or more tables based on a related column between them.
```sql
SELECT "name", "title" FROM "authors"
JOIN "authored" ON "authors"."id" = "authored"."author_id"
JOIN "books" ON "books"."id" = "authored"."book_id";
```
<p align="center"><img width="278" alt="Screenshot 2023-10-13 at 11 51 12 AM" src="https://github.com/CodexploreRepo/sql/assets/64508435/fe668b74-ed29-4083-9add-194db870be8c"></p>


### What is View ?
- A view is a virtual table defined by a query and it does not consume much more disk space to create.
  - The data within a view is still stored in the underlying tables, but still accessible through this simplfied view.
- For example: we wrote a query to join three tables, as in the previous example, and then select the relevant columns. The new table created by this query can be saved as a view, to be further queried later on.
- Views are useful for:
  - **simplifying**: putting together data from different tables to be queried more simply,
  - **aggregating**: running aggregate functions, like finding the sum, and storing the results,
  - **partitioning**: dividing data into logical pieces,
  - **securing**: hiding columns that should be kept secure. While there are other ways in which views can be useful, in this lecture we will focus on the above four.
 
## `CREATE VIEW`
- To save the virtual table created in the previous step as a view:
```sql
CREATE VIEW "longlist" AS
SELECT "name", "title" FROM "authors"
JOIN "authored" ON "authors"."id" = "authored"."author_id"
JOIN "books" ON "books"."id" = "authored"."book_id"
ORDER BY "title";
```
- The view created here is called longlist. This view can now be used exactly as we would use a table in SQL.
```sql
SELECT "title" FROM "longlist" WHERE "name" = 'Fernanda Melchor';
```
- Each time a view is created, it gets added to the schema.
### `CREATE TEMPORARY VIEW`
- To create temporary views that are not stored in the database schema, we can use `CREATE TEMPORARY VIEW`.
- This command creates a view that exists only for the duration of our connection with the database.
- temporary views are used when we want to organize data in some way without actually storing that organization long-term.
```sql
CREATE TEMPORARY VIEW "average_ratings_by_year" AS
SELECT "year", ROUND(AVG("rating"), 2) AS "rating" FROM "average_book_ratings" 
GROUP BY "year";
```
## Common Table Expression (CTE)
- A regular view exists forever in our database schema.
- A temporary view exists for the duration of our connection with the database.
- A **CTE** is a view that **exists for a single query alone**.

```sql
WITH "average_book_ratings" AS (
    SELECT "book_id", "title", "year", ROUND(AVG("rating"), 2) AS "rating" FROM "ratings"
    JOIN "books" ON "ratings"."book_id" = "books"."id"
    GROUP BY "book_id"
)
SELECT "year" ROUND(AVG("rating"), 2) AS "rating" FROM "average_book_ratings"
GROUP BY "year";

```
