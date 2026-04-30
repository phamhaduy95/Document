#### TRUNCATE TABLE

`TRUNCATE TABLE` is a SQL command that quickly removes **all rows** from a table, resetting it to an empty state while keeping the table's structure (columns, indexes, constraints) intact.
it's much faster than `DELETE` for large tables because it's a `DDL` operation that doesn't log individual row deletions, but it's often non-transactional (cannot be rolled back) and resets auto-increment counters, requiring caution and proper permission

PostgreSQL enable rollback for table truncate operation
#### DROP TABLE

With PostgreSQL, adding a column to a table and filling it with values can quickly inflate the table’s size because the database creates a new version of the existing row each time a value is updated, but it doesn’t delete the old
row version.


