IAM quản lý quyền access và sử dụng các tài nguyên và dịch vụ trong AWS.
Principal:  IAM user, resource hoặc application có quyền truy cập vào AWS resource.
Các principal trong AWS

- **IAM user** -
- **IAM group** - danh sách các IAM user sẽ thừa hưởng 1 IAM policy chung
- **IAM role** - cấp quyền truy cập cho 1 resource đến các resource khác ( ví dụ tạo role cấp quyền cho EC2 instance get data từ DynamoDB)

##### Root user

root user là quyền truy cấp cao nhất đối với 1 AWS account. AWS cung cấp quyền nay cho AWS account.

Note: the root user is not an IAM user controlled by IAM security.

root user nên được dùng để thực hiện các tác vụ billing, changing the AWS support plan, or reviewing tax invoices.

best practice với root user:

- tìm và xóa tất cả các access key sinh ra từ root user.
- disabled access key generation từ root account
- Thay vì dùng root user, ta nên tạo 1 IAM user có `AdministratorAccess`permission đảm trách administration task.

Các task chỉ root user mới có quyền thực hiện:

- Thay đổi các thông tin, password của root account
- Xóa AWS account
- Thay đổi AWS support plan tier (free, developer, business, enterprise)
- Enabling billing for the account or changing your payment options or billing information  
- Creating a CloudFront key pair  
- Enabling MFA on an S3 bucket in your AWS account  
- Requesting permission to perform a penetration test  
- Restoring IAM user permissions that have been revoked

##### IAM User

Mỗi 1 account có thể tạo ra nhiều IAM user. Ta có thể tạo IAM user trên AWS console bằng root account.

IAM áp dụng **least privilege policy** tức IAM *identity* khi mới tạo mặc định sẽ không được cấp quyền access vào AWS resource nào. Để thêm quyền cho một IAM *identity*, ta có attach 1 policy.

Khi tạo mới 1 IAM user, ta có thể:

- Áp đặt password strength thông qua password policy: container certain set of letters, password expiration và no password duplication.
- define quyền access lên các AWS resource thông IAM policy
- access key: tạo access key để sử dụng AWS SDK và AWS CLI.
- access key rotation: enable key rotation giúp refresh access key sau một khoảng thời gian sử dụng.

##### IAM User group

IAM group bao gồm danh sách nhiều IAM users. Ta có thể assign 1 IAM policy lên IAM group để moi user trong group đều được thừa hường policy đó.
Một user có thể thuộc nhiều group khác nhau
- > A group is not an identity and cannot be identified as a principal in an IAM policy
Only users and services can assume a role to take on permissions (not groups)
#### IAM policy

IAM policy là JSON file mô tả các quyền truy cập và sử dụng (**permission**) cho AWS resource

IAM policy có định dạng JSON bao gồm các field sau đây:

- **Version** (mandatory) version của JSON file sử dụng cho IAM policy
- **Statement** (mandatory):
 	- **Effect**: gồm 2 giá trị `Allow` và `Deny`. `Deny` thường được dùng để override các quyền đã explicit `Allow`
 	- **Action**: các tác vụ mà user có thể thực hiện. IAM chỉ chấp nhật các action hợp lệ đã được define sẵn.
 	- **Resource**: AWS resources that the actions in the statement apply to
 	- **Condition**: điều kiện để policy được áp dụng.

``` JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAllEC2ActionsOnSpecificInstance",
      "Effect": "Allow",
      "Action": [
        "ec2:*"
      ],
      "Resource": "arn:aws:ec2:your-region:your-account-id:instance/*",
      "Condition": {
        "StringEquals": {
          "aws:ResourceTag/Name": "critical-web-server"
        }
      }
    },
    {
      "Sid": "AllowDescribeOnAllInstances",
      "Effect": "Allow",
      "Action": [
        "ec2:Describe*"
      ],
      "Resource": "*"
    }
  ]
}
```

The evaluation logic of IAM policies follows these strict rules:  

- By default, all requests are implicitly denied; there are no implicit permissions. Actions are not allowed without an explicit allow.  
- Policies are evaluated for an explicit deny; if found the action is denied.  
- An explicit allow overrides the implicit deny, allowing the action to be carried out.  
- An explicit deny denies a requested action.

AWS có các loại IAM policy chính sau:

- **AWS managed policies** - AWS cung cấp các policy sẵn mà user có thể tái sử dụng cho nhiều identity khác nhau.

* **customer-managed policies**  -  reusable như managed policies, tuy nhiên user có quyền custom mà manage (delete, update, create).

- **inline policies** - policy associate với 1 identity duy nhất. Khi identity đó bị removed, in-line policy cũng sẽ bị remove theo.
- **resource-base policies** - policy associate với 1 AWS resource. Chỉ 1 vài resource hỗ trợ policy này (S3, SQS, ...)

##### Permission boundary

You can apply a permission boundary policy for both the IAM user and IAM role within a single AWS account. Without a permission boundary being defined, the applied managed or custom policy defines the maximum permissions that are granted to each particular IAM user or role.

``` json
{

    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:*",
                "ec2:*"
            ],
            "Resource": "*"
        }
    ]
}
```

##### amazon resource name

`arn:partition:service:region:account-id:resource-type/resource-id`

##### Condition

| Element                      | Description                                                                                          |
| ---------------------------- | ---------------------------------------------------------------------------------------------------- |
| `aws:CurrentTime`            | This element checks for date/time conditions.                                                        |
| `aws:SecureTransport`        | The request must use Secure Sockets Layer (SSL/TLS).                                                 |
| `aws:UserAgent`              | This element allows certain client applications to make requests.                                    |
| `aws:MultiFactorAuthPresent` | With this element, you can use the `BoolIfExists` operator to deny requests that do not include MFA. |
| `Bool`                       | The value of this element must be `true`.                                                            |
| `StringEquals`               | The request must contain a specific value.                                                           |
| `aws:PrincipalOrgID`         | With this element, the user must be a member of a specific AWS organization.                         |
| `aws:PrincipalTag/tag-key`   | This element checks for specific tags.                                                               |
| `aws:RequestTag/tag-key`     | This element checks for a tag and a specific value.                                                  |
| `aws:PrincipalType`          | This element checks for a specific user or role.                                                     |
| `aws:SourceVpce`             | This element restricts access to a specific endpoint.                                                |
| `aws:RequestedRegion`        | This element allows you to control the regions to which API calls can be made.                       |
| `aws:SourceIp`               | This element specifies an IPv4 or IPv6 address or range of addresses.                                |
| `aws:userid`                 | This element checks the user’s ID.                                                                   |

##### IAM Role

An IAM role cung cấp **temporary access** vào AWS resources cho các AWS resource khác như EC2, IAM user thuộc AWS account khác hoặc external user authenticated dùng SAML 2.0 hoặc OpenID connect.

Khi ta tạo 1 IAM role mới,  AWS sẽ tự động kèm theo 1 **trust policy** quy định các **identity** có thể được assume role.

Ta có thể assume một role có sắn cho một resource nhất định. Tuy nhiên ta cần lưu ý kiểm tra lại trong **trust policy** có cho phép assume role cho resource đó ko. Nếu ko có, ta cần edit file trust policy.

``` JSON
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

authentication và authorization cho IAM roles được quản lý và thực hiện bởi AWS Security Token Service (STS)

##### Best practice

The best way to protect your root account is to lock it down by doing the following:  

- Delete any access keys associated with root.  
- Assign a long and complex password and store it in a secure password vault.  
- Enable multifactor authentication (MFA) for the root account.  
- Wherever possible, don’t use root to perform administration operations
- Never store your access keys and secret key in ec2 instances or any other cloud storage. If you need to access AWS resources from an ec2 instance, use IAM ec2 roles.

trên IAM Dashboard, ta có thể download 1 CSV Credential Report danh sách các IAM user và các thông tin đăng nhập như access key status.

You can allow or disallow the ability to change passwords using an IAM policy and you should attach this to the group that contains the users
