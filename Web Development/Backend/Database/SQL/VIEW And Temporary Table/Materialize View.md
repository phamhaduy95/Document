evaluate query và save result trong disk , thay vì thực thi on the fly khi được sử dụng.
Materialize View giúp filter trước 1 câu query tốn kém và reuse khi cần thiết
Đây là một 

They are ideal for read-heavy operations, dashboards, and reporting where data does not need to be real-time

Trade off:  khi muốn update query, ta phải drop view và recreate lại 1 view mới.
