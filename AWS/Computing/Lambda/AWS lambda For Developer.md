#### Lambda Input argument
Khi Lambda function được  executed, input được truyền vào function gồm 2:
- `event`:  chứa các data liên quan đến event truyền vào 
- `context`: chứa runtime metadata của lambda evocation như
	- `context.aws_request_id`
	- `context.memory_limit_in_mb`
	- `context.function_name`
#### Version and Alias

Khi ta muốn thay đổi code, environment variables hoặc các configuration của lambda function, ta cần tạo và publish 1 version mới. Khi 1 version mới được published lên, version mới sẽ được đánh tag là $LATEST. Lưu ý $LATEST chưa chắc là version đã deploy. Để chính thức deploy một version bất kỳ, ta cần phải update trong các file template trên CloudFormation hoặc SAM.

version có hai dạng chính:
- phiên bản số tăng dần
- LATEST phiên bản mới nhất

ARN trỏ đến lambda function với từng version khác nhau.
**Base function ARN**: `arn:aws:lambda:us-east-1:1234567890:function:my-function`
**Specific version ARN**: `arn:aws:lambda:us-east-1:1234567890:function:my-function:1`
**`$LATEST` version ARN**: `arn:aws:lambda:us-east-1:1234567890:function:my-function:$LATEST`

Ngoài ra ta có thể đặt 1 tên có ý nghĩa hơn chỏ đến version của lambda function sử dụng alias
ARN của từng alias có dạng giống như sau:
`arn:aws:lambda:us-east-1:1234567890:function:my-function:PROD`

Ta có thể tạo nhiều alias khác nhau và cung cấp weight để điều hướng traffic đến từng alias ứng theo tỉ lệ tương ứng. Kết hợp với các service AWS Step Function ta có thể thiết lập 1 process canary deployment.

Ví dụ: Ta có 2 phiên bản lambda function có alias lần lượt là PROD-OLD và PROD-NEW. Ta điều chỉnh traffic cho PROD-OLD là 90% và PROD-NEW là 10% 
``` bash
aws lambda create-alias \
    --function-name my-function \
    --name PROD-OLD \
	--function-version 1 \
    --routing-config '{"AdditionalVersionWeights": {"1": 0.9}}' \
    --description "Alias for live production with 90% traffic to version 1"

```

``` bash
aws lambda create-alias \
    --function-name my-function \
    --name PROD-NEW \
    --function-version 2 \
    --routing-config '{"AdditionalVersionWeights": {"2": 0.1}}' \
    --description "Alias for live production with 10% traffic to version 2"

```
#### Environment Variable
Ta có thể tạo và thêm các biến môi trường cho 1 lambda function cho phép user access các biến trong lambda handler. 
Ví dụ. Ta có thể thêm 1 biến môi trường vào 1 lambda function như sau:

``` bash
aws lambda update-function-configuration \
    --function-name my-function \
    --environment '{"Variables": {"API_URL": "https://api.example.com", "LOG_LEVEL": "INFO"}}'
```

Ta có thể access biến môi trường trong lambda function như sau

``` javascript
exports.handler = async (event) => {
    // Access the stage variables directly from the event object
    const stageVariables = event.stageVariables || {};
    const envName = stageVariables.ENV_NAME || 'default_value';

    console.log(`Environment Name from Stage Variable: ${envName}`);
    // ... your function logic
    
    const response = {
        statusCode: 200,
        body: JSON.stringify(`Accessed stage variable ENV_NAME: ${envName}`),
    };
    return response;
};
```

Lưu ý Không nên lưu các thông tin nhạy cảm như credential, API key thông qua environment variable. Thay vào đó ta nên dùng AWS secret manager hoặc AWS parameter store
#### Lambda layers

Ta có thể tạo và đóng gói một shared module cho phép nhiều lambda trong một execution environment. 

Layers thường được dùng với các mục đích sau:
- Giảm package size cho lambda function do ta tách riêng các external module vào layers
- Tạo 1 share module custom runtime cho nhiều lambda sử dụng

Khi ta tạo 1 layer, các file trong layer sẽ được lưu trong directory `/opt` trong execution environment.

Các bước đóng gói và deploy 1 lambda layer:

- **step 1**: package các file và directory của module trong 1 file zip. Để giúp lambda dễ dàng tìm kiếm và import được module trong layer ta cần tuân thủ về quy tắc tên cũng như structure cho từng ngôn ngữ như sau

| Rune Time | Folder Name                                      | Example Content       |
| --------- | ------------------------------------------------ | --------------------- |
| Python    | `python` or `python/lib/python3.x/site-packages` | `requests/`, `numpy/` |
| Node.js   | `nodejs` or `nodejs/node_modules`                | `uuid/`, `axios/`     |
| Java      | `java` or `java/lib`                             | `.jar` files          |
- **step 2**: deploy layer thông qua CLI lên AWS .  Layer sẽ được lưu trong S3. Command sẽ trả về ARN cho layers.

``` bash
aws lambda publish-layer-version \
    --layer-name MyLambdaLayer \
    --description "My dependencies" \
    --content S3Bucket=my-s3-bucket,S3Key=my_layer.zip \
    --compatible-runtimes python3.10 nodejs18.x

```

- **step 3**: assign layer đó vào 1 function:
- 
``` bash
aws lambda update-function-configuration \
    --function-name my-function \
    --layers arn:aws:lambda:us-east-1:123456789012:layer:MyLambdaLayer:1
```

Quota:
User có thể cung cấp tối đa 5 layer cho 1 lambda function.
Layer có kích thước tối đa là 250Mb và 50Mb khi đã compress.
#### Lambda Invocation

Ta có thể invoke lambda theo các cách sau đây:
##### synchronous invocation

program sẽ chờ lambda function thực thi xong và kết quả trả về (blocking mode). ta có thể call lambda trước tiếp dùng asynchronous mode thông qua API `Invoke`.

synchronous invoke thường được dùng trong mục đính testing là chính. Không nên áp dụng trong code thực tế do blocking mode ảnh hưởng đến performance.

``` Bash
aws lambda invoke \
    --function-name my-function \
    --cli-binary-format raw-in-base64-out \
    --payload '{"key": "value", "name": "test"}' \
    response.json

```

---cli-binary-format: 
##### asynchronous mode 
Mode mặc định của lambda function cho nhiều service như S3, SNS, ...
Ta có thể thêm  `--invocation-type=Event` để lambda được execute asynchronous.

``` shell
aws lambda invoke \
    --function-name my-async-function \
    --invocation-type Event \
    --payload '{"key": "value", "action": "process_image"}' \
    response.json

```
##### event source mappings
Đối với các service dạng streaming hoặc queue như Kinesis hoặc SQS, Lambda có sử dụng cơ chế polling 1 số lượng record và gom chúng lại thành 1 batch rồi sử lý. 

lambda áp dụng  _event source mapping_  cho các AWS service sau đây:

- DynamoDB streaming
- Kinesis 
- SQS
- AWS MQ

**IAM permission**
Your Lambda function's execution role needs permissions to read data from the event source:

- **For SQS:** Attach the `AWSLambdaSQSQueueExecutionRole` managed policy or equivalent custom permissions (e.g., `sqs:ReceiveMessage`, `sqs:DeleteMessage`, `sqs:GetQueueAttributes`).
- **For DynamoDB/Kinesis Streams:** Attach the `AWSLambdaDynamoDBExecutionRole` or `AWSLambdaKinesisExecutionRole` managed policy (e.g., `dynamodb:GetRecords`, `dynamodb:GetShardIterator`, `dynamodb:DescribeStream`, `dynamodb:ListStreams`).

 **event source mapping creation**
Ta có thể tạo 1 event-source mapping cho lambda function như sau: 

 Example for SQS
 ``` bash
 aws lambda create-event-source-mapping \
    --function-name YourFunctionName \
    --event-source-arn arn:aws:sqs:us-east-1:123456789012:MyQueue \
    --batch-size 10 \
    --query UUID --output text
 ```

 Example for Kinesis/DynamoDB Streams
 ``` bash
 aws lambda create-event-source-mapping \
    --function-name YourFunctionName \
    --event-source-arn arn:aws:kinesis:us-east-1:123456789012:stream/MyStream \
    --batch-size 100 \
    --starting-position TRIM_HORIZON 
    # TRIM_HORIZON starts reading from the oldest available record
 ```

Các thông số quan trọng
- **BatchSize:** The maximum number of records or messages Lambda reads from the source and sends to your function in a single invocation.
- **MaximumBatchingWindowInSeconds:** The maximum amount of time (0 to 300 seconds) Lambda spends gathering records before invoking the function.
- **Starting Position (Streams only):** Specifies where to begin reading from a stream (e.g., `TRIM_HORIZON` for oldest data, `LATEST` for new data, or `AT_TIMESTAMP`).
- **FilterCriteria (SQS only):** [Control which events Lambda sends to your function](https://docs.aws.amazon.com/lambda/latest/dg/invocation-eventfiltering.html)

#### warm execution environment 

AWS invoke các lambda function trong 1 execution environment. Để tối ưu hóa và giảm thiểu chi phí, AWS vẫn duy trì execution environment trong khoảng thời gian ngắn để tận dụng cho các function tiếp theo. 

Các tài nguyên ta có thể tận dụng bao gồm.
- các variable được define ngoài lambda handler. Ví dụ reuse database connection  
- các file hoặc data được lưu trong `/temp` storage.
- cached value được lấy từ AWS Secret Manager hoặc AWS Parameter Storage
- các shared module của layers được lưu trong `/opt` folder

#### Tips
Optimize External API Calls
Use persistent HTTP connections (e.g.,  
using Connection: keep-alive) to avoid the overhead of establishing a new  
connection for each request. Implement exponential backoff and retry logic  
for handling transient errors when making external API calls to reduce the  
risk of throttling or failed requests.