**Amazon CloudWatch Logs** is a **fully managed log storage and monitoring service** that lets you:

- **Collect**, **store**, and **analyze** log data from AWS services or your own applications.
- **Search**, **filter**, and **create metrics** from logs in near real time.
- **Trigger alarms or automated actions** based on log data.

#### Core component

|**Component**|**Description**|
|---|---|
|**Log Event**|A single line of log data (timestamp + message).|
|**Log Stream**|A sequence of log events from the same source (e.g., one EC2 instance, one Lambda function).|
|**Log Group**|A container for multiple log streams (e.g., `/aws/lambda/myFunction`, `/app/backend/api`).|
|**Subscription Filter**|Streams logs to another service (e.g., Lambda, Kinesis, or Firehose) for processing.|
|**Metric Filter**|Creates **CloudWatch metrics** from specific patterns found in logs.|
|**Retention Policy**|Determines how long to keep logs (1 day – indefinite).|
|**Log Insights**|Query and analyze logs using SQL-like syntax.|
#### Metric Filters

You can extract metrics from log patterns and trigger alarms. 
Example: Create a metric `ErrorCount` that increments each time “ERROR” appears.  
You can then create a **CloudWatch Alarm** on that metric.

Note : CloudWatch Logs metric filters only apply to _newly ingested log events_, not existing (old) logs.


#### Subscription Filters

You can **stream logs** in real time to other services:

- **AWS Lambda** — for custom log analysis or alerting.
- **Amazon Kinesis Stream or Firehose** — for real-time processing or export to S3.
- **OpenSearch Service (Elasticsearch)** — for indexing and searching.

#### Log retention

Các data được log bởi CloudWatch là immutable, user ko thể manually remove. Để tránh làm phình bộ nhớ, CloudWatch có cơ chế gom các data có resolution cao thành bộ data có resolution thấp hơn sau 1 khoảng thời gian nhất định.

- data point có interval 1s như high-resolution custom metric chỉ tồn tại không quá 3 giờ trước khi CloudWatch tự động thành gom lại thành  data point interval 1 phút (lấy average)
- data point có interval 1 phút như detailed monitoring tồn tại trong 15 ngày.
- data point có interval 5 phút tồn tại trong 63 ngày và sẽ gom thành 1 giở.


Permission
IAM permissions for `logs:CreateLogGroup`, `logs:PutLogEvents`, etc.