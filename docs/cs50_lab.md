# CS50 Lab
## Preparation
- Log into cs50.dev using your GitHub account
- Course materials: [Notes](https://cs50.harvard.edu/sql/2023/)
- Run `update50` in your codespace’s terminal window
- Open the sqlite3 with longlist.db: `sqlite3 longlist.db`
### SQLite3
- `.quit` to quit the terminals
- `.tables` to list down all the tables in the schema
- `.schema` SQLite command (not an SQL keyword) that can shed more light on how this database was created.
  - `.schema books`  to see the schema for a specified table `books`
- `.read schema.sql` to read and create the schema based on the script `schema.sql`
  - Method 1: `.import --csv --skip 1 file_name.csv sql_table_name` to insert data from csv to table, `skip 1` is to skip the header
  - Method 2:
    - Import csv file as temp sql_table: `.import --csv file_name.csv temp`
    - Insert the record from temp table to the main table:
    ```sql
    INSERT INTO "collections" ("title", "accession_number", "acquired") 
    SELECT "title", "accession_number", "acquired" FROM "temp";

    -- drop temp table once done
    DROP TABLE "temp";
    ```
  
