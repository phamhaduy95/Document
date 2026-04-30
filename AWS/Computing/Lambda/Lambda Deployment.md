#### Package And Deploy Lambda Function

Ta có thể package Lambda dưới 2 định dạng sau:

- **zip file:** kích thước package chưa zip là 250Mb. Cách phổ biến để package
- **container image:** package size tối đa  tăng là 10G đổi lại cold start cao hơn rất nhiều. Phú hợp với các ứng dụng không cần response cao như worker task, background task.

Các bước để tạo và deploy 1 lambda lên AWS:
- **Step 1 (packaging step)**:  ta tạo 1 folder chung chứa function code và toàn bộ các dependency và compress folder đó lại dưới dạng zip hoặc image.
- **Step 2**: deploy lambda function thông qua CLI

``` bash
aws lambda create-function \
    --function-name MyNewZipFunction \
    --runtime python3.9 \
    --role arn:aws:iam::123456789012:role/lambda-basic-execution \
    --handler lambda_function.lambda_handler \ # trỏ đến file và function có handler của lambda chính
    --zip-file fileb://function.zip 
    # fileb:// specifies a local binary file path
```

Ngoài cách deploy trực tiếp thông qua CLI, ta có thể deploy lambda function thông qua những cách sau:

- Deploy lambda thông qua AWS CloudFormation
- Deploy lambda thông qua Serverless Application Model (SAM)
- Deploy lambda thông CI/CD tool như AWS CodeDeploy hoặc AWS CodePipeLine

#### Deploy Lambda with SAM and CloudFormation

Quá trình deploy một lambda function thông qua SAM diễn ra như sau: 
**Step 1**:  Khởi tạo 1 SAM project và include code của lambda function vào trong project directory

``` 
sam-lambda-api/
├── template.yaml
└── src/
    └── app.py
```

**Step 2**:  Tạo file `template.yaml` và mô tả lambda configuration trong phần `Resources`. Template hỗ trợ mọi configuration quan trọng của lambda function như memory, function name, timeout, ...

``` yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Full example showing all major Lambda configurations in SAM

Globals:
  Function:
    Runtime: python3.9
    Timeout: 10
    MemorySize: 256

Resources:
  MyLambdaFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: MyLambdaFunction
      Handler: app.lambda_handler
      CodeUri: src/
      
      # ─────────────────────────────
      # BASIC CONFIGURATION
      # ─────────────────────────────
      Description: "Example Lambda with full configuration"
      MemorySize: 512                     # MB
      Timeout: 15                         # seconds
      ReservedConcurrentExecutions: 5     # concurrency limit
      EphemeralStorage:                   # /tmp storage (default 512MB)
        Size: 1024                        # can go up to 10,240 MB
      
      # ─────────────────────────────
      # VPC CONFIGURATION
      # ─────────────────────────────
      VpcConfig:
        SecurityGroupIds:
          - sg-0123456789abcdef0
        SubnetIds:
          - subnet-0123456789abcdef0
          - subnet-abcdef0123456789
      
      # ─────────────────────────────
      # ENVIRONMENT VARIABLES
      # ─────────────────────────────
      Environment:
        Variables:
          STAGE: prod
          LOG_LEVEL: INFO
          DB_HOST: mydb.cluster-xxxx.us-east-1.rds.amazonaws.com
      
      # ─────────────────────────────
      # PERMISSIONS / POLICIES
      # ─────────────────────────────
      Policies:
        - AWSLambdaBasicExecutionRole
        - AWSLambdaVPCAccessExecutionRole
        - DynamoDBCrudPolicy:
            TableName: MyTable
        - Statement:
            - Effect: Allow
              Action: s3:GetObject
              Resource: arn:aws:s3:::mybucket/*
      
      # ─────────────────────────────
      # TRACING, LOGGING, AND MONITORING
      # ─────────────────────────────
      Tracing: Active                  # enables AWS X-Ray
      Layers:
        - arn:aws:lambda:us-east-1:123456789012:layer:MySharedLayer:3
      
      # ─────────────────────────────
      # EVENT SOURCES (EXAMPLE)
      # ─────────────────────────────
      Events:
        ApiEvent:
          Type: Api
          Properties:
            Path: /hello
            Method: get


```

>[!note] lưu ý
>Global field quy định common configuration cho toàn bộ lambda function. Ta có thể override các value này trong từng definition của từng function

**Step 3**: Build project bằng command `sam build`. SAM build sẽ tiến hành generate ra CloudFormation Template tương ứng. 

**Step 4**: prompt command `sam deploy --guided`. parameter `--guided` sẽ hướng dẫn người dùng nhập thêm các thông tin quan trong như AWS region, stack name,...

Ngoài source code tại local, SAM còn cho phép user import lambda function từ các nguồn khác:
**S3 bucket:** pre-built package dưới dạng zip bao gồm code và dependency.
``` yaml
CodeUri:
  Bucket: my-lambda-code-bucket
  Key: app/latest.zip

```

**Container Images**: 
```yaml
Type: AWS::Serverless::Function
Properties:
  PackageType: Image
  ImageUri: <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-lambda:latest
```
**inline code**: phù hợp cho function nhỏ.
```yaml
Type: AWS::Lambda::Function
Properties:
  Runtime: python3.9
  Handler: index.handler
  Role: arn:aws:iam::123456789012:role/LambdaRole
  Code:
    ZipFile: |
      def handler(event, context):
          return "Hello inline!"
```

#### Deploy lambda with AWS CodeDeploy

When we previously delved into AWS CodeDeploy, it was through the lens of code running on traditional server-based compute environments. However, CodeDeploy also allows developers to choose AWS Lambda as a deploy target, giving you the ability to automate your Lambda releases.

After you create a CodeDeploy application with compute type AWS Lambda, you then create a deployment group. Unlike with EC2 deployments, there are no instances to select, as the destination is the Lambda service itself. However, you still select the deployment configuration—the pattern of how your new code will roll out. You have three categories from which to choose: canary, linear, or all-at-once, as shown in the following selected examples:

- `**CodeDeployDefault.LambdaCanary10Percent5Minutes**`  For five minutes, Code-Deploy will route 10 percent of invocations to your new code before cutting all traffic over entirely. This window gives you time to monitor CloudWatch logs and cancel the deployment if any errors occur. This can be done manually or through a post-deployment hook in your `appspec.yml` file.
- `**CodeDeployDefault.LambdaLinear10PercentEvery10Minutes**`  CodeDeploy will ramp up the number of invocations that are handled by the new code, starting at 10 percent and increasing by an additional 10 percent every 10 minutes, until all traffic is handled by the newest version.
- `CodeDeployDefault.LambdaAllAtOnce`: All invocations of your Lambda function will immediately be handled by the newest code version.
