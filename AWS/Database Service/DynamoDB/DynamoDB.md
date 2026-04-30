#### Introduction

AWS cung cấp dịch schemaless (NoSQL) database với hiệu năng, tính mở rộng và availability cao. DynamoDB hỗ trợ lưu trữ dữ liệu theo định dạng key-value hoặc Document.

Vì là serverless, dynamoDB sẽ thừa hưởng những điểm mạnh đặc thù của một hệ thống serverless:

- **high scalability:** storage và performance sẽ scale theo nhu cầu sử dụng.
- **high availability:** có nhiều bản sao lưu ở 3 AZ khác nhau, cho phép tạo các cluster phụ tại nhiều region.
- **consistent high performance:**  response luôn ổn định (vài millisecond) ở tất cả các mức scale khác nhau, DynamoDB có zero cold start.
#### Performance

Hiệu năng của **dynamoDB** sẽ phụ thuộc vào các 2 thông số RCU và WCU được cung cấp:

- **Read capacity unit (RCU)** – One strongly consistent read per second, or two eventually consistent reads per second, for items up to 4 KB in size.
- **Write capacity unit (WCU)** – One write per second, for items up to 1 KB in size.

**DynamoDB** cung cấp hai phương án sau để user điều chỉnh hiệu năng cho DB của mình:
- **on demand (DynamoDB AutoScaling)**: DynamoDB sẽ linh hoạt thay đổi RCU vả WCU theo nhu cầu sử dụng.
- **provisioned**: cho phép user đặt một số lượng cố định cho RCU và WCU. User còn có thể giảm sâu chí phí bằng việc preserve RCU và WCU từ 1 đến 3 năm. Lưu ý, AWS vẫn charge tiền cho số lượng RCU và WCU đã đặt ngay ca khi không sử dụng chí vì thế phương án này chỉ phù hợp với các ứng dụng có mức hoạt động ổn định.
- provision autoscaling: ta có thể define 1 range min-max cho RCU và WCU
#### High Availability
DynamoDB có những tính năng giúp gia tăng availability của hệ thống:

1. data được sao lưu thành 6 bản sao ở 3 AZ khác nhau.
2. dùng Global tables tạo các replica ở nhiều region.

#### strong consistency vs eventually consistency

Do DynamoDB tạo nhiều data replica ở ba AZ khác nhau, quá trình cập nhật dữ liệu cho DynamoDB sẽ cần một khoảng thời gian để toàn bộ replica được cấp nhật. Chính vì thế, khi ra thực hiện 1 lệnh read sau ngay khi một lệnh write, data trả về có thể là data cũ. Hiện tượng này có tên gọi là **eventually consistency**, do DynamoDB sẽ lấy data từ 1 replica bất kỳ mà không quan tâm data ở replica đó đã được cập nhật mới chưa.

Để khác phụ vấn đề trên ta có thể tăng WCU và RCU hoặc enable **strongly consistency** để DynamoDB chỉ serve data từ replica đã được update thành công.


![[Aurora Storage.png]]

#### Global Table
Global table cho phép tạo nhiều replica ở nhiều region khác nhau cho phép tự động failover khi có sự cố. 
Global table áp dụng **active-active replication,** tức secondary cluster ở các region phụ vẫn hoạt động và phục vụ read và write request bình thường như DB chính. Điều này cho phép tăng hiệu năng và giảm latency cho các end-user ở từng region. 
#### Transaction

support ACID transaction:
100 actions per 1 transaction
With the transaction write API, you can group multiple `Put`, `Update`, `Delete`, and `ConditionCheck` actions.
These actions can target up to 100 distinct items in one or more DynamoDB tables within the same AWS account and in the same Region. The aggregate size of the items in the transaction cannot exceed 4 MB.
#### Pricing
pricing theo *pay-per-use* model, dựa vào 2 tiêu chí sau:
lượng data được lưu trữ trong DynamoDB (tính theo Gig)
số lương write compute unit (WCU) và read compute unit (RCU) được sử dụng
#### Integration Services

**DynamoDB Accelerator (DAX)** is a fully managed, highly available caching service cho dynamoDB
**DynamoDB zero-ETL** integration with:
- **Amazon SageMaker Lakehouse** eliminates the need to build custom data movement pipelines by automatically replicating DynamoDB data to Amazon SageMaker Lakehouse
- **Amazon Redshift** within a few minutes of data being written in DynamoDB
#### Table limits
- **Number of tables** – For any AWS account, there is an initial quota of 2,500 tables per AWS Region.
- **Page size limit for query and scan** – There is a limit of 1 MB per page, per query or scan. If your query parameters or scan operation on a table result in more than 1 MB of data, DynamoDB returns the initial matching items. It also returns a `LastEvaluatedKey` property that you can use in a new request to read the next page.

You can store JSON files up to 400KB in size in a DynamoDB table

