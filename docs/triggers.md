# Triggers
## What is SQL Trigger ?
- A trigger is a stored procedure in a database that automatically invokes whenever a special event in the database occurs.
- For example: we want to delete a record in `collections` table, say `id=4`, and the deleted record should be updated in `transactions` table
<p align="center"><img width="500" alt="Screenshot 2023-10-13 at 11 18 52 AM" src="https://github.com/CodexploreRepo/sql/assets/64508435/9f965496-4e12-4f6a-9d27-651700c4f7b3">
</p>

- After the deletion from `collections` table, the deleted item should be updated in `transactions` table & marked as SOLD
<p align="center"><img width="500" alt="Screenshot 2023-10-13 at 11 19 37 AM" src="https://github.com/CodexploreRepo/sql/assets/64508435/580c99f4-2c88-44ac-8129-6eaacb55993f"></p>


## Trigger Syntax
```sql
CREATE TRIGGER [trigger_name] 
[BEFORE | AFTER | INSTEAD OF]  {INSERT | UPDATE | DELETE}  ON [table_name]  
FOR EACH ROW                           -- This specifies a row-level trigger, i.e., the trigger will be executed for each affected row.
BEGIN 
[trigger_body]
END
```
- Ex 1: If we delete an item on "collections" table, create a trigger to update that item into the `transactions` table & marked as SOLD
```sql
CREATE TRIGGER "sell"                 -- the trigger's name is SELL
BEFORE DELETE ON "collections"
FOR EACH ROW
BEGIN
    INSERT INTO "transactions" ("title", "action")
    VALUES (OLD."title", 'sold');     -- OLD refers to the deleted row in "collection" table
END;
```
- Ex 2: If we insert an item on "collections" table, create a trigger to update that item into the `transactions` table & marked as BOUGHT
```sql
CREATE TRIGGER "buy"                 -- the trigger's name is BUY
AFTER INSERT ON "collections"
FOR EACH ROW
BEGIN
    INSERT INTO "transactions" ("title", "action")
    VALUES (NEW."title", 'bought');     -- NEW refers to the newly inserted row in "collection" table
END;
```
### Triggers used in Soft Deletion 
-  A soft deletion involves marking a row as deleted instead of removing it from the table.
-  For example: a piece of art called “Farmers working at dawn” is marked as deleted from the `collections` table by changing the value in the deleted column from 0 to 1.
<p align="center"><img width="258" alt="Screenshot 2023-10-13 at 4 16 56 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/63c62d95-ec87-4631-94e2-bc8f9fd7ca60"></p>

- We can create a view to display information about the rows that are not deleted.
```sql
CREATE VIEW "current_collections" AS
SELECT "id", "title", "accession_number", "acquired" 
FROM "collections" 
WHERE "deleted" = 0;
```
- Now, we have the underlying `collections` table where it contains both current & deleted items, where the `current_collections` contains only current items.
<p align="center"><img width="500" alt="Screenshot 2023-10-13 at 4 19 30 PM" src="https://github.com/CodexploreRepo/sql/assets/64508435/fc3ee0c8-1a2d-4835-8a06-91e69959a83a"></p>

- For the case of deleting & inserting in the view, we already know that it is not possible to insert data into or delete data from a view. However, we can set up a trigger that inserts into or deletes from the underlying table! The INSTEAD OF trigger allows us to do this.
- Deleting Trigger
```sql
CREATE TRIGGER "delete"
INSTEAD OF DELETE ON "current_collections"
FOR EACH ROW
BEGIN
    UPDATE "collections" SET "deleted" = 1 
    WHERE "id" = OLD."id";
END;

-- Now, we can delete from the view
DELETE FROM "current_collections" 
WHERE "title" = 'Imaginative landscape';
```
- Inserting trigger: there are two situations to consider here.
- Situation 1: We could be trying to insert into a view a row that already exists in the underlying table, but was soft deleted. We can write the following trigger to handle this situation.
```sql
CREATE TRIGGER "insert_when_exists"
INSTEAD OF INSERT ON "current_collections"
FOR EACH ROW 
WHEN NEW."accession_number" IN (                    -- WHEN keyword is used to check if the accession number of the artwork already exists in the collections table. 
    SELECT "accession_number" FROM "collections"
)
BEGIN
    UPDATE "collections" 
    SET "deleted" = 0 
    WHERE "accession_number" = NEW."accession_number";
END;
```
- Situation 2: The second situation occurs when we are trying to insert a row that does not exist in the underlying table. The following trigger handles this situation.
```sql
CREATE TRIGGER "insert_when_new"
INSTEAD OF INSERT ON "current_collections"
FOR EACH ROW
WHEN NEW."accession_number" NOT IN (
    SELECT "accession_number" FROM "collections"
)
BEGIN
    INSERT INTO "collections" ("title", "accession_number", "acquired")
    VALUES (NEW."title", NEW."accession_number", NEW."acquired");
END;
```

