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

## Inserting Data
- The SQL statement `INSERT INTO` is used to insert a row of data into a given table.
```sql
-- CREATE TABlE Schema
CREATE TABLE "collections" (
    "id" INTEGER,
    "title" TEXT NOT NULL,
    "accession_number" TEXT NOT NULL UNIQUE,
    "acquired" NUMERIC,
    PRIMARY KEY("id")
);
-- INSERT
INSERT INTO "collections" ("id", "title", "accession_number", "acquired")
VALUES (1, 'Profusion of flowers', '56.257', '1956-04-12');
```
- SQL can fill out the primary key values automatically. To make use of this functionality, we omit the ID column altogether while inserting a row.
  - If we delete a row with the primary key 1, SQL will not automatically assign a primary key of 1 to the next inserted row.
    - Instead it actually selects the highest primary key value in the table and increments it to generate the next primary key value.
```sql
INSERT INTO "collections" ("title", "accession_number", "acquired")
VALUES ('Farmers working at dawn', '11.6152', '1911-08-03'); -- the "id" will be auto-populated as 2
```
### Inserting Multiple Rows
- `INSERT INTO ... VALUES (row1), (row2), (row3);` inserting multiple rows at once in this manner allows the programmer some convenience.
```sql
INSERT INTO "collections" ("title", "accession_number", "acquired") 
VALUES 
('Imaginative landscape', '56.496', NULL),
('Peonies and butterfly', '06.1899', '1906-01-01');
```

## Deleting Data
- Delete all the rows that are already within the `collections` table.
```sql
DELETE FROM "collections";
```
- We can also delete rows that match specific conditions.
```sql
DELETE FROM "collections"
WHERE "title" = 'Spring outing';

DELETE FROM "collections"
WHERE "acquired" IS NULL;

DELETE FROM "collections"
WHERE "acquired" < '1909-01-01';

DELETE FROM "created"
WHERE "artist_id" = (
    SELECT "id"
    FROM "artists"
    WHERE "name" = 'Unidentified artist'
);
```

### Cascade Deletion
- For example: if we choose to delete the **unidentified artist** (with the ID 3), what would happen to the rows in the table `created` with an `artist_id` of 3? 
<p align="center">
<img width="524" alt="Screenshot 2023-10-12 at 9 29 21 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/0380390e-a286-496b-b220-535e03d1878d"></p>

- On running below command, we get `Runtime error: FOREIGN KEY constraint failed (19)`.
  - This error notifies us that deleting this data would violate the foreign key constraint set up in the `created` table.
```sql
DELETE FROM "artists"
WHERE "name" = 'Unidentified artist';
```

- SOLUTION: specify the action to be taken when an ID referenced by a foreign key is deleted. To do this, we use the keyword `ON DELETE` followed by the action to be taken.
  - `ON DELETE RESTRICT`: This restricts us from deleting IDs when the foreign key constraint is violated.
  - `ON DELETE NO ACTION`: This allows the deletion of IDs that are referenced by a foreign key and nothing happens.
  - `ON DELETE SET NULL`: This allows the deletion of IDs that are referenced by a foreign key and sets the foreign key references to NULL.
  - `ON DELETE SET DEFAULT`: This does the same as the previous, but allows us to set a default value instead of NULL.
  - `ON DELETE CASCADE`: This allows the deletion of IDs that are referenced by a foreign key and also proceeds to cascadingly delete the referencing foreign key rows.
    - For example, if we used this to delete an artist ID, all the artist’s affiliations with the artwork would also be deleted from the created table.

```sql
-- add the below code when creating the "created" table 
FOREIGN KEY("artist_id") REFERENCES "artists"("id") ON DELETE CASCADE

-- Now running the following DELETE statement will not result in an error, and will cascade the deletion from the artists table to the created table:
DELETE FROM "artists"
WHERE "name" = 'Unidentified artist';
```

