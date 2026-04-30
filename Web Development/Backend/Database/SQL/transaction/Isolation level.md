
A page : a fixed-size segment of memory used by database to store its records (its size varies from 2Kb- 16Kb)

Locking

While multiple users can read data simultaneously, only one write
lock is given out at a time for each table (or portion thereof), and read requests
are blocked until the write lock is released.