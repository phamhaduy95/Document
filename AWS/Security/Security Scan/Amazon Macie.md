#### Introduction
security service áp dụng Machine Learning tìm kiếm các sensitive data trong các S3 object.

Các loại sensitive data bao gồm:

- Credential data such as private keys or AWS secret access keys  
- Credit card and bank account numbers
- Personal information, health insurance details, passports, and medical IDs  

 Ngoài ra, ta có thể cung cấp một custom identifiers sử dụng regular expressions (regex) hay keyword.

Ngoại trừ customer-provided keys (SSE-C), Amazon Macie có thể phân tích encrypted S3 bucket sử dụng phương thức bảo mật khác như **KMS** hoặc **S3-KMS**
#### Integration

Amazon Macie publishes near-real-time logging data cho CloudWatch logs

Amazon Macie can trigger event cho **Amazon EventBridge** 
