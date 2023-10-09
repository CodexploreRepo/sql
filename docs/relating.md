# Relating

## Entity Relationship Diagrams
- There are one-to-one, one-to-many and many-to-many relationships between tables in a database.
- It is possible to visualize such relationships using an **entity relationship (ER)** diagram.
  - Each table is an entity in our database. The relationships between the tables, or entities, are represented by the verbs that mark the lines connecting entities.
<p align="center"><img width="249" alt="Screenshot 2023-10-07 at 10 36 57 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/52d069ad-ca97-4001-809c-0c073719918b">
</p>

- *Example of ER diagram*:
  - Ex 1: an author writes at least one book and a book is written by at least one author.  
<p align="center"><img width="360" alt="Screenshot 2023-10-07 at 10 38 14 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/fa219b71-c5cf-4908-a956-6b27f7b95baa">
</p>

<p align="center">
<img width="518" alt="Screenshot 2023-10-07 at 10 40 37 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/29706a17-4967-4a73-a9ea-f533c1b435be">
</p>

## Keys
### Primary Keys
- **Primary key** is an identifier that is unique for every item in a table.
- For example: in the case of books, every book has a unique identifier called an ISBN

### Foreign Keys
- **Foreign key** is a primary key taken from a different table. By referencing the primary key of a different table, it helps relate the tables by forming a link between them.
- Ex 1 (one-to-many): primary key of the `books` table is now a column in the `ratings` table.
  - This helps form a one-to-many relationship between the two tables — a book with a title (found in the `books` table) can have multiple ratings (found in the `ratings` table). 
<p align="center"><img width="485" alt="Screenshot 2023-10-07 at 11 14 40 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/ef47e29f-2c37-4dda-9dba-59bd75a9fc76"></p>

- Ex 2 (many-to-many relationship): a table called `authored` that maps the primary key of `books` (book_id) to the primary key of `authors` (author_id)
<p align="center"><img width="525" alt="Screenshot 2023-10-07 at 11 21 05 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/e46176d6-f43b-4557-bf12-867771d86c16"></p>

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
