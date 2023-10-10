# Relating

## Entity Relationship (ER) Diagrams
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

### Translating ER diagram to SQL tables
- For example: MRT system, our entities (riders and stations) are related. A rider will likely visit multiple stations, and a subway station is likely to have more than one rider. Given this, it will be a many-to-many relationship.
<p align="center">
<img width="125" alt="Screenshot 2023-10-10 at 10 33 47 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/421af2e2-9a10-4d99-b7c6-e7f139953bcc">
</p>

- SQL commands to create the above schema
```sql
CREATE TABLE riders (
    "id" INTEGER,
    "name" TEXT
);

CREATE TABLE stations (
    "id" INTEGER,
    "name" TEXT,
    "line" TEXT
);
-- "visits" relationship is converted to a table
CREATE TABLE visits (
    "rider_id" INTEGER,
    "station_id" INTEGER
);
```
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

## Normalizing
- For example: This table contains subway rider names, current stations the riders are at and the action performed at the station (like entering and exiting).
  - It also records the fares paid and balance amounts on their subway cards.
  - It also contains an ID for each rider “transaction”, which serves as the primary key.
<p align="center">
<img width="401" alt="Screenshot 2023-10-10 at 10 15 49 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/e6308948-3a7e-4878-954c-2cd111d83fb7">
</p>

- What redundancies exist in this table?
  - We may choose to separate out rider names into a table of its own, to avoid having to duplicate the names so many times.
    - We would need to give each rider an ID that can be used to relate the new table to this one.
  - We may similarly choose to move subway stations to a different table and give each subway station an ID to be used as a foreign key here.
- The process of separating our data in this manner is called `normalizing`.
  - When normalizing, we put each entity in its own table—as we did with riders and subway stations.
  - Any information about a specific entity, for example a rider’s address, goes into the entity’s own table.
 
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
