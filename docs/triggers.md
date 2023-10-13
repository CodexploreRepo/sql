# Triggers
## What is SQL Trigger ?
- A trigger is a stored procedure in a database that automatically invokes whenever a special event in the database occurs.
- For example: we want to delete a record in `collections` table, say `id=4`, and the deleted record should be updated in `transactions` table
<p align="center"><img width="500" alt="Screenshot 2023-10-13 at 11 18 52 AM" src="https://github.com/CodexploreRepo/sql/assets/64508435/9f965496-4e12-4f6a-9d27-651700c4f7b3">
</p>

- After the deletion from `collections` table, the deleted item should be updated in `transactions` table & marked as SOLD
<p align="center"><img width="500" alt="Screenshot 2023-10-13 at 11 19 37 AM" src="https://github.com/CodexploreRepo/sql/assets/64508435/580c99f4-2c88-44ac-8129-6eaacb55993f"></p>

- Trigger Syntax:
```sql
CREATE TRIGGER "sell"
BEFORE DELETE ON "collections"
FOR EACH ROW
BEGIN
    INSERT INTO "transactions" ("title", "action")
    VALUES (OLD."title", 'sold'); -- OLD refers to the deleted row in "collection" table
END;
```
