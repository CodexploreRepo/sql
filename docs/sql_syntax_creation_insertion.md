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
