We can add constraints in two ways: as a column constraint or as a table
constraint.

#### Primary Key Constraint
composite primary key by combining driver_id with a column holding the state name, which would give us a unique combination for each row primary constraint will be made as an index by default.
### Foreign key constraint

``` SQL
CREATE TABLE registrations (
registration_id text,
registration_date date,
license_id text REFERENCES licenses (license_id) ON
DELETE CASCADE,
CONSTRAINT registration_key PRIMARY KEY (registration_id,
license_id)
);
```

Deleting a row in licenses should also delete all related rows in registrations.

A column that is referenced by a foreign key must have an associated unique constraint.
In our case, the id column of the products.catalog table is a primary key
#### The CHECK Constraint

A CHECK constraint sẽ kiểm tra tính đúng đắn cho một row được inserted hoặc updated trên  database.

CHECK constraint thường được áp dụng để tạo 1 lớp bảo mật data ngay tại tầng database layer khi database được dùng chung bởi nhiều application khác  nhau.

Trong trường hợp chỉ có duy nhất 1 application sử dụng database, ta nên đưa các data validation business logic trên ta application. 

#### The UNIQUE Constraint
The Database Engine automatically creates a UNIQUE index to enforce the uniqueness requirement of the UNIQUE constraint. Therefore, if an attempt to insert a duplicate row is made, the Database Engine returns an error message that states the UNIQUE constraint has been violated and does not add the row to the table. Unless a clustered index is explicitly specified, a unique, non-clustered index is created by default to enforce the UNIQUE constraint.

khi ta muốn enforce mỗi email 1 user


