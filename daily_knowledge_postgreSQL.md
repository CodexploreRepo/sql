# PostgreSQL
## Table Creation
- Error `PostgreSQL Database directory appears to contain a database; Skipping initialization`
  - &#8594; need to proactively remove the volumes which were set up to store the database.
- Error `ERROR:  syntax error at or near "AUTO_INCREMENT"`
  - There is no `auto_increment` in PostgreSQL. Use `SERIAL` instead.
- MUST NOT HAVE `,` in the last col, or else will causing error

### `CREATE TABLE`
```sql
# Incorrect postgreSQL syntax
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    firstname VARCHAR(100) NOT NULL,
    surname VARCHAR(100) NOT NULL, # this comma will cause the error
);

# Correct postgreSQL syntax
CREATE TABLE students (
    id SERIAL PRIMARY KEY,
    firstname VARCHAR(100) NOT NULL,
    surname VARCHAR(100) NOT NULL # need to remove the comma here
);
```

### `INSERT INTO`
- The values must be in single quote `'` instead of double quote `"`
```sql
INSERT INTO students (firstname, surname)
VALUES ('John', 'Andersen'), ('Ha Quan', 'Nguyen');  
```
