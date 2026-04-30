Locking
tại sao ta cần locking: locking giúp tránh hiện tượng race condition cho database.
locking đảm bảo write operation chỉ xảy ra trên 1 phạm vị trong 1 thời điểm.
khi locking, nếu database sử dụng pessimistic locking, read cũng không được thực hiện tại row hay table được lock. Nhược điểm , thời gian chờ lâu.


ngoài ra ta còn có optimistic locking hay versioning, read user vẫn truy vấn được data. Tuy nhiên nếu lượng concurrency lớn, vẫn xảy ra hiện tượng data inconsistency  khi read data đó lên

purpose of transaction

mổ ta các tiêu chuẩn của ACID

why we need to perform a transaction

row-level lock does not block reading but writing instead


Isolation level

|Isolation Level|Dirty Read|Non-Repeatable Read|Phantom Read|
|:--|:--|:--|:--|
|Read uncommitted|Possible|Possible|Possible|
|Read committed|Not possible|Possible|Possible|
|Repeatable read|Not possible|Not possible|Possible|
|Serializable|Not possible|Not possible|Not possible|
Postgres offers the read committed and serializable isolation levels.

MMVC create a tuple for each transaction to ensure isolation both uncommitted and committed data  
_Read Committed_ is the default isolation level in Postgres. When a transaction runs on this isolation level, a **SELECT** query sees only data committed before the query began and never sees either uncommitted data or changes committed during query execution by concurrent transactions. (However, the **SELECT** does see the effects of previous updates executed within this same transaction, even though they are not yet committed.)

if two transaction try to update the same row, postgre will serialize 
update require row-level lock