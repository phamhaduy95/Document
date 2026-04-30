#### Introduction

Amazon API Gateway làm 1 managed serverless service từ AWS thường được dùng làm layer trung gian giữa client với các Backend service.

Các vai trò nổi bật của API Gateway bao gồm:
- proxy cho các Backend service (liên kết thông qua VPC PrivateLink)
- hoặc tạo các HTTP endpoint trigger các lambda function
- hỗ trợ integration với các external HTTP endpoint.

Ngoài ra còn hỗ trợ thực hiện các tính năng:
- authentication and authorization
- redirect và rewrite request
- caching response
- throttling hay rate limit request
- hỗ trợ retry mechanism và canary deployment
- thiết lập stage deployment (DEV, STAGING, PROD) cho các endpoints

API gateway hỗ trợ các chuẩn format sau:
- REST API: hỗ trợ đầy đủ các tính năng và integration với nhiều AWS service. 
- HTTP API: phiên bản đơn giản it tính năng và rẻ hơn so với REST API. Chỉ hỗ trợ Lambda và HTTP endpoint
- Web socket cho các ứng dụng real time streaming hoặc duplex communication 

Đối với GraphQL API ta nên sử dụng AWS AppSync để xây dựng
#### Deployment Mode

API Gateway hỗ trợ 3 loại deployment mode quyết tính đến độ phủ sóng accessibility như sau:
- **Regional Endpoints**  Deployed inside a specific AWS region, such as `us-west-2`, Regional endpoints may experience additional latency when called by clients outside the region.
- **Edge-Optimized Endpoints**  Edge-optimized endpoints leverage Amazon CloudFront’s network of global distribution points to provide connection points for clients so that wherever your consumers are in the world, they can access your API with an endpoint that is geographically close and low-latency.
- **Private Endpoints**: Endpoints phục vụ nội bộ trong private VPC network
#### Integration



AWS Gateway integrates với nhiều AWS service khác tạo nên bộ công cụ mạnh mẽ xây dựng serverless application.
- **AWS Lambda**: tạo 1 REST hay HTTP endpoint forward request tới 1 lambda function và nhận data trả về từ lambda function. Một trong những cách phổ biến để xây dựng serverless app.
- **AWS Step Functions**:  Trigger một Step Function workflow 
- **Amazon DynamoDB**: You can use API Gateway to create a direct integration that interacts with DynamoDB without needing a Lambda function. For example, a `GET` request could be mapped directly to a `GetItem` action in DynamoDB.
- **Amazon Kinesis**: stream real-time data từ web Socket của API gateway đến amazon kinesis  
- **Amazon SQS**: Send API requests to an SQS queue for asynchronous processing by other services.
- **Amazon VPC**: Private integrations allow API Gateway to securely connect to resources inside your Amazon Virtual Private Cloud (VPC), such as EC2 instances or containers, without exposing them to the public internet.

