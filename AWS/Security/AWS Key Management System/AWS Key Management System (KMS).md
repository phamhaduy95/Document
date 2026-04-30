#### Introduction

AWS Key Management Service (KMS) is a fully managed service that allows you to create, manage, and control cryptographic keys used for encrypting and signing data across AWS services and applications

Lưu ý:
KMS là region-locked service nên scope của 1 key chỉ nằm trong 1 region. Tuy nhiên ta có thể tạo 1 replicated key sang region khác và share cho account khác 
#### Features

- AWS Key Management Service is central storage which keeps all keys used by many services in AWS such as EBS, S3,...
- You can ask KMS issue new key or import your own key called **customer-managed keys** (CMKs)
- **Customer-managed keys** can be shared with multiple AWS accounts.
- You can also create a **multi-region key** has the same key material and key ID in different Regions, which allows data encrypted in one Region to be decrypted in another without needing a cross-Region KMS call. 
- Support **key rotation and lifecycle**
#### Application

- Encrypt and decrypt data at rest inside storage such as EBS, RDS, ... 
- perform client-side encryption for data in transit via encryption SDK 
- perform digital signing operations with asymmetric keys (e.g., ECC, RSA) to validate data authenticity, useful for code signing or API responses.




