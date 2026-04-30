


The new `shared_buffers` value increased the memory available to Postgres for buffers (blocks
read from disk and/or used in operations), `work_mem` increased the amount of non-shared memory available to individual operations, and `maintenance_work_mem` increased the memory available for operations like CREATE INDEX or VACUUM.

to spill temporary data into discs which lower the performance significantly (up to x2 slower)

Fortunately, work_mem can be SET at the session level, so we can configure the system with some sensible default and only increase work_mem for those queries that actually need the
extra memory to run.

`SET LOCAL work_mem = '256MB';` for a specific `SELECT` statement that needs lots of memory for sorting



maintain task includes:
- vacuum

partition 

checking index usage
Partitioning by multiple keys is not the same as multi-level
partitioning.