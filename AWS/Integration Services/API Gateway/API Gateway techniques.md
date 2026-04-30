#### Authentication
API Gateway cung cấp nhiều phương pháp authentication và authorization để bảo vệ endpoint.
##### 1. lambda authorizer
Tạo 1 endpoint dùng 1 lambda function đặc biệt gọi là lambda authorizer và trả về 1 JSON dùng IAM policy template để mô tả permission execute 1 hay nhiều endpoint trong API gateway.

```JSON
{
  "principalId": "yyyyyyyy", 
  "policyDocument": {
    "Version":"2012-10-17",		 	 	 
    "Statement": [
      {
        "Action": "execute-api:Invoke",
        "Effect": "Allow|Deny",
        "Resource": "arn:aws:execute-api:{regionId}:{accountId}:{apiId}/{stage}/{httpVerb}/[{resource}/[{child-resources}]]"
      }
    ]
  },
  "context": {
    "stringKey": "value",
    "numberKey": "1",
    "booleanKey": "true"
  },
  "usageIdentifierKey": "{api-key}"
}

```

##### 2. resource-base policy
Ta có thể tạo 1 resource base policy và attach cho từng endpoint trong API gateway
Ví du ta tạo 1 policy chỉ cho phép request từ 2 IP address được gọi lambda.

``` JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "execute-api:Invoke",
            "Resource": [
                "arn:aws:execute-api:us-east-1:*:api-id/*"
            ],
            "Condition": {
                "IpAddress": {
                    "aws:SourceIp": [
                        "192.0.2.0/24",
                        "203.0.113.0/24"
                    ]
                }
            }
        },
    ]
}

```

Sau đó ta có thể attach vào 1 endpoint thông qua CLI  như sau
``` bash
aws apigateway create-rest-api \
    --name "api-name" \
    --policy "{\"jsonEscapedPolicyDocument\"}"
```
  
Enable Cross-Account Access: To grant another AWS account access to your API, include the account ID in the Principal element of the policy
##### 3. token-base authentication
Ta có thể thông qua 1 identity provider như AWS Cognito 


| Value                | Description                                                            | Typical Use Case                                                       |
| -------------------- | ---------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `NONE`               | No authentication. Anyone with the endpoint URL can call it.           | Public APIs, health checks                                             |
| `AWS_IAM`            | Requires the caller to sign requests with **AWS credentials (SigV4)**. | Private APIs for internal AWS apps or signed SDK requests              |
| `CUSTOM`             | Uses a **Lambda authorizer (custom authorizer)** that you define.      | Fine-grained access control using custom tokens (e.g., JWTs, API keys) |
| `COGNITO_USER_POOLS` | Uses an **Amazon Cognito user pool authorizer**.                       | Authentication via Cognito login tokens (JWTs from user pool)          |

#### API caching
Ta có thể bật tắt tính năng caching cho từng endpoint bằng việc điều chỉnh thời gian caching (Time To Live) thông qua header `Cache-Control:max-age`. TTL có giá trị trong khoảng từ 0 đến 3600s
Để disable caching ta set HTTP header `Cache-Control:max-age=0`

User có thể đều chỉnh cách AWS caching cho từng endpoint của mình như phân biết method, parameter, URL

API gateway caching không phải cách tối ưu để caching API do API gateway không bắt được khi nào data trên DB thay đổi. Ta cần kết hợp với ElasticCache with Redis để tận dụng write-through caching mechanism.

Ví dụ về chỉnh caching cho API gateway
https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-caching.html#override-api-gateway-stage-cache-for-method-cache


#### Stage deployment


`{api-id}.execute-api.{region}.amazonaws.com/{stage}/{method}`

In the pets example, when deploying to a stage called `dev`, the URL to retrieve all pets would look like this:

```
GET q83ij7fn1.execute-api.us-east-1.amazonaws.com/dev/pets
```

Stage variables allow you to parameterize certain elements of your deployment with values set at the stage level. One use case described by AWS is making your backend function references dynamic such that different stages refer to different function versions. Recall that a Lambda function can be tagged with version labels or aliases, which can be specified with a colon at the end of its ARN:

```
arn:aws:lambda:{region}:{account-id}:function:{function-name}:{version-or-
```

#### API rate limit

Amazon API Gateway provides four basic types of throttling-related settings:
cho phép ta d
limit cho toàn bộ accounts và user
limit cho từng account trên 1 region:  
cho từng endpoint

Trong trường hợp user

A _usage plan_ specifies who can access one or more deployed API stages and methods—and optionally sets the target request rate to start throttling requests

The plan uses API keys to identify API clients and who can access the associated API stages for each key


Các setup rate limit cho user 
[https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-create-usage-plans-with-console.html](https://repost.aws/questions/QUtgY5LtcqSXeoeozinhDCrw/how-to-implement-rate-limiting-in-api-gateway-per-user)

#### API gateway CORS

To use Amazon API Gateway, you must enable the CORS _resource_ in the Amazon API Gateway console so that your web application makes calls to the Amazon API Gateway service successfully. Without CORS, any calls made to the Amazon API Gateway service will fail.

pre-flight call đến backend server để kiểm tra 
#### Logging with CloudWatch
Các metric quan trọng cần quan tâm
`4XXError`: The number of client-side errors captured in a specified period
`CacheHitCount`: The number of requests served from the API cache in a given period
`CacheMissCount`:  The number of requests served from the back end in a given period, when API caching is enabled
`IntegrationLatency`: The time between when Amazon API Gateway relays a request to the backend and when it receives a response from the backend

#### Open API extension
Ta có thể cung cấp 1 spec OpenAPI (swagger) có sẵn cho API gateway tiến hành xây dựng các endpoint tương ứng theo thiết kế.
https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-swagger-extensions.html


You can integrate an API method in your API Gateway with a custom HTTP endpoint of your application in two ways:  

- HTTP proxy integration  

- HTTP custom integration In your API Gateway console, you can define the type of HTTP integration of your resource by toggling the "Configure as proxy resource" checkbox.    

With proxy integration, the setup is simple. You only need to set the HTTP method and the HTTP endpoint URI, according to the backend requirements, if you are not concerned with content encoding or caching

[https://docs.aws.amazon.com/apigateway/latest/developerguide/setup-http-integrations.html](https://docs.aws.amazon.com/apigateway/latest/developerguide/setup-http-integrations.html)



set up VPC links enable you to create private integrations that connect your HTTP API routes to private resources in a VPC, such as Application Load Balancers or Amazon ECS container-based applications. To learn more about creating private integrations


#### proxy vs non-proxy Integration
integration có 2 kiểu:


proxy setup:
- **Simplified Setup:** API Gateway acts as a direct proxy, forwarding the entire HTTP request (headers, body, query parameters, path variables) as a single JSON `event` object to the Lambda function. The configuration data can include current deployment stage name, stage variables, user identity, or authorization context (if any)
- **Full Control within Lambda:** The Lambda function is responsible for parsing the incoming request and constructing the entire HTTP response (status code, headers, body).
- **Rapid Development:** This approach simplifies API development as less configuration is required in API Gateway.
    
custom setup:
- **Granular Control in API Gateway:** API Gateway explicitly defines how to map incoming request parameters to the Lambda function's input payload and how to map the Lambda function's output to the HTTP response. 
	- Sử dụng mapping template 
    
- **Transformation Capabilities:** API Gateway can transform the request and response payloads using mapping templates (e.g., Velocity Template Language - VTL).
    
- **Decoupled Responsibilities:** he Lambda function focuses solely on business logic, while API Gateway handles the HTTP details and data transformations.


Private

step to define API and associate resource to it

Use the following [create-rest-api](https://docs.aws.amazon.com/cli/latest/reference/apigateway/create-rest-api.html) command to create an API:
``` bash
aws apigateway create-rest-api --name 'HelloWorld (AWS CLI)'
```

Use the following [create-resource](https://docs.aws.amazon.com/cli/latest/reference/apigateway/create-resource.html) command to create an API Gateway [Resource](https://docs.aws.amazon.com/apigateway/latest/api/API_Resource.html)

```bash
	aws apigateway create-resource \
      --rest-api-id te6si5ach7 \
      --parent-id krznpq9xpg \
      --path-part greeting
```

  Use the following [put-method](https://docs.aws.amazon.com/cli/latest/reference/apigateway/put-method.html) command to create an API method request of `GET /greeting?greeter={name}`
  
``` bash
aws apigateway put-method --rest-api-id te6si5ach7 \
       --resource-id 2jf6xt \
       --http-method GET \
       --authorization-type "NONE" \
       --request-parameters method.request.querystring.greeter=false
```


``` bash
aws apigateway put-integration \
        --rest-api-id te6si5ach7 \
        --resource-id 2jf6xt \
        --http-method GET \
        --type AWS \
        --integration-http-method POST \
        --uri arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:123456789012:function:HelloWorld/invocations \
        --request-templates '{"application/json":"{\"greeter\":\"$input.params('greeter')\"}"}' \
        --credentials arn:aws:iam::123456789012:role/apigAwsProxyRole
```

``` bash
aws apigateway create-deployment \
        --rest-api-id te6si5ach7 \
        --stage-name test
```



.  
Enable Cross-Account Access: To grant another AWS account access to  
your API, include the account ID in the Principal element of the policy