database migration
- những lưu ý khi thực hiện database migration.
- migration tool 
- fill data vào các column mới

phương pháp backup và recovery


performance query
tìm bottleneck performance cho các câu query


 integers are cheaper to compare than characters because character sets and collations.  you should store dates and times in MySQL’s built-
in types instead of as strings, and you should use integers for IP addresses

why choosing correct column type is matter
INT 
a DATETIME and a TIMESTAMP column can store the same kind of data:
date and time, to a precision of one second. However, TIMESTAMP uses only half as
much storage space, is time zone aware, and has special auto-updating capabilities.
On the other hand, it has a much smaller range of allowable values, and sometimes its special capabilities can be a handicap

VAR and VARCHAR
CHAR is fix-sized string while VARCHAR is variable string

CHAR is useful if you want to store very short strings or if all the values are nearly
the same length. For example, CHAR is a good choice for MD5 values for user pass‐
words, which are always the same length. 


The tool merely makes the process less impactful and does not
require disruptive write locks

cẩn thận với NULL value

hàm COUNT() hoặc các hàm aggregate khác đều ignore NULL value
