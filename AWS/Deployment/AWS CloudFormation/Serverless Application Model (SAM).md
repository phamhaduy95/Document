[Deploying serverless applications gradually with AWS SAM - AWS Serverless Application Model](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/automating-updates-to-serverless-apps.html)


AWS Serverless Application Model (SAM)  
AWS SAM is an open-source framework designed specifically for building  serverless applications. It simplifies the process of defining serverless  
architectures by allowing developers to use simple, declarative syntax in  
YAML to define AWS resources like Lambda functions, API Gateway APIs,  
DynamoDB tables, and more.

SAM sử dụng format template của CloudFormation

### Deployment Steps


#### SAM Template
SAM template là 1 extension của CloudFormation template thừa hưởng tất cả tính năng vá format của CloudFormation template. Điểm khác biệt duy nhất là tên service khác cho lambda, API gateway và DynamoDB

Trong 1 số trường hợp, infra stack bao gồm các serverless services quản lý bởi SAM và các infra thông thường như EC2, RDS. 
Ta có thể define cả SAM và CloudFormation trong cùng 1 file bằng cách thêm `Transform` trong file template

``` yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Combined CloudFormation and SAM template

Resources:
  MyBucket:
    Type: AWS::S3::Bucket

  MyLambda:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: src/
      Handler: app.lambda_handler
      Runtime: python3.9
      Policies:
        - S3ReadPolicy:
            BucketName: !Ref MyBucket
```


Hoặc dùng Nested stack

SAM template

``` YAMl
AWSTemplateFormatVersion: '2010-09-09'
Description: Parent stack deploying a SAM child stack
Resources:
  ServerlessApp:
    Type: AWS::CloudFormation::Stack
    Properties:
      TemplateURL: https://s3.amazonaws.com/my-bucket/sam-template.yaml
      Parameters:
        Environment: prod

```

Main template
``` yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: Combined CloudFormation and SAM template

Resources:
  MyBucket:
    Type: AWS::S3::Bucket

  MyLambda:
    Type: AWS::Serverless::Function
    Properties:
      CodeUri: src/
      Handler: app.lambda_handler
      Runtime: python3.9
      Policies:
        - S3ReadPolicy:
            BucketName: !Ref MyBucket
```


