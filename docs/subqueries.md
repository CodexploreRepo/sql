# Subqueries
## What is subquery ?
- A subquery is a query inside another query. These are also called nested queries.
```sql
SELECT "title"
FROM "books"
WHERE "publisher_id" = (
    SELECT "id"
    FROM "publishers"
    WHERE "publisher" = 'Fitzcarraldo Editions'
);
```

- The subquery is in parentheses. The query that is furthest inside parantheses will be run first, followed by outer queries.
- The inner query is indented. This is done as per style conventions for subqueries, to increase readability.

## `IN`
- This keyword is used to check whether the desired value is in a given list or set of values.
- For example: The relationship between authors and books is many-to-many. This means that it is possible a given author has written more than one book. To find the names of all books in the database written by Fernanda Melchor, we would use the `IN` keyword as follows.

```sql
SELECT "title"
FROM "books"
WHERE "id" IN (
    SELECT "book_id"
    FROM "authored"
    WHERE "author_id" = (
        SELECT "id"
        FROM "authors"
        WHERE "name" = 'Fernanda Melchor'
    )
```
