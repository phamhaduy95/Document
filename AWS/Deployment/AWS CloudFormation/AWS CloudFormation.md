#### Core concept

| Concept             | Description                                                 | Example                                     |
| ------------------- | ----------------------------------------------------------- | ------------------------------------------- |
| **Template**        | YAML/JSON file defining resources and configuration.        | `template.yaml`                             |
| **Stack**           | A deployed instance of a template.                          | “Production stack”                          |
| **Change Set**      | Preview of what changes will occur before updating a stack. | "Update Preview"                            |
| **Stack Policy**    | Restricts updates to certain resources.                     | Protect an S3 bucket from replacement       |
| **Drift Detection** | Detects manual changes made outside CloudFormation.         | “Someone modified my EC2 instance manually” |
|                     |                                                             |                                             |

##### Stack 
tập hợp hay unit of deployment cho toàn bộ các resource như EC2, RDS. 
Ta có thể tao 1 stack, delete stack hoặc update stack 

#### Change Set
Change Set giúp user nhận biết được các resource nào được thay đổi trong 1 stack trước khi apply 1 update

Ví dụ. Ta update 1 CloudFormation template của 1 stack. Ta sẽ tạo 1 change set bằng CLI
``` bash
aws cloudformation create-change-set \
  --stack-name my-app-stack \
  --template-body file://updated-template.yaml \
  --change-set-name my-change-set \
  --description "Update EC2 instance type and add an S3 bucket"

```

Sau đó ta tiến hành hiên thị change set qua terminal 
``` bash
aws cloudformation describe-change-set \
  --change-set-name my-change-set \
  --stack-name my-app-stack
```

Khi kiểm tra xong, ta có thể apply change set và delete

``` bash
aws cloudformation execute-change-set \
  --change-set-name my-change-set \
  --stack-name my-app-stack

```

``` bash
aws cloudformation delete-change-set \
  --change-set-name my-change-set \
  --stack-name my-app-stack
```


#### Step for deploying CloudFormation

CloudFormation template sử dụng các file hoặc resource tại local như lambda function, nested stack. Ta cần package các local resource này và lưu lên trên S3.

step 1: package 
``` bash
aws cloudformation package \
  --template-file template.yaml \
  --s3-bucket my-artifact-bucket \
  --output-template-file packaged.yaml
```

Uploads local artifacts (Lambda code, layers, etc.) to Amazon S3.
Replaces local paths with **S3 URLs** in the output template.
Produces a new CloudFormation-compatible template (e.g., `packaged.yaml`).


step 2: Deploy
``` bash
aws cloudformation deploy \
  --template-file packaged.yaml \
  --stack-name my-serverless-app \
  --capabilities CAPABILITY_IAM

```

- Creates or updates the CloudFormation stack.
- Deploys all serverless resources (Lambda, API Gateway, DynamoDB, etc.).
- Handles IAM roles automatically if `CAPABILITY_IAM` or `CAPABILITY_NAMED_IAM` is provided.


#### Multiple Stage deployment

tạo nhiều deployment stack cho nhiều môi trường khác nhau
Công cụ


