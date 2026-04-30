subquery 

query syntax

SELECT first_name

query subset of columns 
ORDER BY field_name DESC

import from external data such as `CSV`

``` SQL
COPY table_name
FROM 'C:\YourDirectory\your_file.csv'
WITH (FORMAT CSV, HEADER);
```

``` SQL
COPY us_counties_pop_est_2019
TO 'C:\YourDirectory\us_counties_export.txt'
WITH (FORMAT CSV, HEADER, DELIMITER '|');
```

import subset of column from db
create table with desire schema

CREATE TEMPORARY TABLE supervisor_salaries_temp
(LIKE supervisor_salaries INCLUDING ALL);

COPY supervisor_salaries_temp (town, supervisor, salary)
FROM 'C:\YourDirectory\supervisor_salaries.csv'
WITH (FORMAT CSV, HEADER);

constraint
- primary-key
- foreign key
- not null
- check





