#### Cross-Account Encryption

**Cross-Region Key Replication** is an **AWS KMS feature** that allows you to automatically **replicate a multi-Region key** from one AWS Region to another

replicated key được tạo ra có các đặc điểm sau:

- **Same key ID and key material**
- **Different regional endpoints**
- **Independent key policies and usage quotas**

This means data encrypted in one region can be decrypted in another.


``` bash
aws kms replicate-key \
  --key-id arn:aws:kms:us-east-1:111122223333:key/abcd1234-5678-90ef-ghij-klmnopqrstuv \
  --replica-region ap-southeast-1

```

