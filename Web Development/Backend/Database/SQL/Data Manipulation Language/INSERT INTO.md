Insert new row


UPSERT  data 

Update if there is an existing row

MySQL
``` SQL
INSERT INTO booksales (title,copies)
VALUES('The Greater Trumps',1)
ON DUPLICATE KEY UPDATE copies = copies+1;
```

PostgreSQL 

ON CONFLICT
``` sql
INSERT INTO booksales (title, copies)
VALUES ('The Greater Trumps', 1)
ON CONFLICT (title) 
DO UPDATE SET copies = booksales.copies + 1;
```
