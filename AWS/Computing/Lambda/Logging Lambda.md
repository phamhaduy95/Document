
#### Logging function call
khi sử dụng các hàm log ra `stdout` như console.log với Javascript hay print với python trong lambda. Các log message sẽ được lưu mặc định trong CloudWatch Logs
#### Logging with AWS X-ray
Ta thường X-ray để 
- Trace requests coming **into** your Lambda function.
- Measure **execution time** and **performance bottlenecks**.
- See **downstream service calls** (like to DynamoDB, SQS, or external APIs).
- Diagnose **errors, cold starts, and latency issues**.

X-ray sẽ collect log cho lambda invocation gồm các thông số số
-  a segment của lambda function invocation
- **Subsegments** for downstream calls (like DynamoDB queries or HTTP requests).
#####  PassThrough mode

In **PassThrough** mode, Lambda **does not create new traces on its own**.  
Instead, it only **continues an existing trace** **if** the request already includes an **X-Ray tracing header** (`X-Amzn-Trace-Id`).

That means:
- If a request comes **from an upstream service** that already has tracing enabled (e.g., API Gateway with X-Ray tracing),  
    → your Lambda **joins** that existing trace, so you get **end-to-end visibility** across services.
- If a request comes **directly** to Lambda **without** a trace header,  
    → Lambda will **not generate a new trace**. It simply **passes through** without tracing.
#### Lambda Destination
Ngoài ra với  asynchronous call và event source mapping, ta có thể dùng tính năng destination cho phép ta lưu các record về lambda invocation. 

Record này chứa các metadata của lambda như execution status (Success/Failure), timestamps, request payload, and response/error details.

``` bash
aws lambda update-function-event-invoke-config \
    --function-name MyTestFunction \
    --destination-config '{"OnSuccess": {"Destination": "arn:aws:sqs:us-east-1:123456789012:MySuccessQueue"}, "OnFailure": {"Destination": "arn:aws:sns:us-east-1:123456789012:MyFailureTopic"}}'
```

Ví dụ về invocation record
``` JSON
{
    "version": "1.0",
    "id": "...",
    "detail-type": "Lambda Function Invocation Result",
    "source": "aws.lambda",
    "account": "...",
    "time": "...",
    "region": "...",
    "resources": ["..."],
    "detail": {
        "functionArn": "...",
        "state": "SUCCESS", // Indicates success
        "responsePayload": { /* The return value of your function */ },
        "requestPayload": { /* The original input event */ },
        "internalSdkVersion": "...",
        "status": "OK"
    }
}

```



