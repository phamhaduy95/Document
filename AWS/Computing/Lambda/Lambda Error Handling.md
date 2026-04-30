#### Error handling
với synchronous event, AWS lambda không thực hiện retry. User tư manually handle error

với asynchronous event, AWS lambda áp dúng exp backoff retry strategy để handle error. Mỗi error bị fail lên tục,  AWS lambda sẽ tăng thời gian delay để

#### dead-letter queue

Ta có thể lưu record về những lần fail lambda  và forward nó đến 1 dead-letter queue trong 1 SQS. 
Lưu ý các event được lưu vào dead letter queue cần thỏa mãn các tiêu chí sau:
event fail hết lượt retry và expired.  

CLI để thêm dead letter queue
``` bash
aws lambda update-function-configuration \
  --function-name my-function \
  --dead-letter-config TargetArn=arn:aws:sns:us-east-1:123456789012:my-topic
```


So sánh giữa destination và dead letter queue

| Feature                        | Destination on Failure                                                | Dead Letter Queue (DLQ)                         |
| ------------------------------ | --------------------------------------------------------------------- | ----------------------------------------------- |
| **Supported Invocation Types** | Asynchronous invocations, SQS, Kinesis, DynamoDB streams              | Asynchronous invocations only                   |
| **Capture Timing**             | **After** all retries (default 2) are exhausted                       | **After** all retries (default 2) are exhausted |
| **Payload/Message Content**    | **Full invocation record** (original event + error details + context) | **Original input event payload only**           |
| **Destination Services**       | SQS, SNS, EventBridge, S3                                             | SQS queue or SNS topic only                     |
| **Configuration Location**     | Function Configuration -> Destinations                                | Function Configuration -> Async invocation      |
| **Recommended Use**            | Preferred method for detailed debugging and robust workflows          | Legacy method, less detailed                    |
|                                |                                                                       |                                                 |
