
Ta có thể share Key trong KMS cho các account khác bằng việc 
- update resource-based policy của key (recommended)
- thêm permission vào IAM policy của account đó

Ví dụ key policy


``` JSON
{
  "Version": "2012-10-17",
  "Id": "key-share-policy",
  "Statement": [
    {
      "Sid": "AllowAccountAFullAccess",
      "Effect": "Allow",
      "Principal": { "AWS": "arn:aws:iam::111111111111:root" },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "AllowAccountBUseOfKey",
      "Effect": "Allow",
      "Principal": { "AWS": "arn:aws:iam::222222222222:root" },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    }
  ]
}
```




