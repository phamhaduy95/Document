#### lambda resource base
Lambda hỗ trợ resource base policy cho phép tạo permission truy cấp đến lambda function đó. Vị dụ.

ta thêm `sns.amazonaws.com`  vào field `principal` cho phép SNS trigger được lambda function

``` bash
aws lambda add-permission \
  --function-name my-function \
  --action lambda:InvokeFunction \
  --statement-id sns \
  --principal sns.amazonaws.com \
  --output text
```

grant access to a Organization

dùng `principal-org-id`

``` bash
aws lambda add-permission \
  --function-name example \
  --statement-id PrincipalOrgIDExample \
  --action lambda:InvokeFunction \
  --principal * \
  --principal-org-id o-a1b2c3d4e5f
 
```

#### share function to other account
Để share function cho 1 AWS account khác ta có add account ID vào principal field của resource base. 

``` bash
aws lambda add-permission \
    --function-name arn:aws:lambda:us-east-1:123456789012:function:MySharedFunction \
    --statement-id AllowAccountBInvoke \
    --action lambda:InvokeFunction \
    --principal 333344445555 \
    --output text
```

Vối các AWS service khác được dùng trong lambda, ta chỉ cần tạo IAM role cho lambda đó không cần tạo permission cho account khác.