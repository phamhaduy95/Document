#### EXPLAIN

https://docs.gitlab.com/development/database/understanding_explain_plans/

PostgreSQL allows you to obtain query plans using the `EXPLAIN` command. This command can be invaluable when trying to determine how a query performs. You can use this command directly in your SQL query, as long as the query starts with it

```sql
EXPLAIN
SELECT COUNT(*)
FROM projects
WHERE visibility_level IN (0, 20);
```

Because `EXPLAIN ANALYZE` executes the query, care should be taken when using a query that writes data or might time out. If the query modifies data, consider wrapping it in a transaction that rolls back automatically like so

``` sql
BEGIN;
EXPLAIN ANALYZE
DELETE FROM users WHERE id = 1;
ROLLBACK;
```