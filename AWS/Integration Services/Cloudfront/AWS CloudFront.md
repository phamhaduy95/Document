#### Introduction

Amazon CloudFront là giải pháp Content Network Delivery (CND) giúp cải thiện serve các static content cho các end user.
CloudFront deliver content thông qua một hệ thống nhiều data center hay edge locations trên toàn thế giới.
Quá trình serve content của CloudFront sẽ diễn ra như sau:

- CloudFront serve file ngay lập tức khi tìm thấy cached file tại edge location
- Nếu không có cached file, CloudFront sẽ download file từ origin và serve cho user đồng thời cache file đó lại tại edge location.
- Ta có thể configure CloudFront cấp nhật cached data tại các edge location khi file được update ở origin.

Các Content origin được support bởi CloudFront:

- **Amazon S3 Bucket**- Any accessible S3 bucket  
- **AWS MediaPackage** - channel endpoint Video packaging and origination  
- **AWS MediaStore** - container endpoint Media-optimized storage service  
- **Application Load Balancer** - Multiple EC2-based web servers  
- **Lambda function** -  URL Serverless workload  
- **Custom origin HTTP server** (even on-premises)

Ngoài ra user sẽ được AWS cung cấp miễn phí một SSL/TLS encryption certificate thông qua AWS Certificate Manager (ACM).

#### Application

**Fast static content delivery**: cache static content tại edge location và serve các cached content cho các user gần edge location để giảm latency.

**Live streaming video**: kết hợp với service Amazon Elastic Transcoder và Amazon Kinesis Video Streams giúp dễ dàng stream video và audio content thông qua các protocol chuyên dụng như HTTP Live Stream (HSL) và Dynamic Adaptive Streaming over HTTP (DASH).

**Enhanced encryption at edge**: hỗ trợ mã hóa và tăng mức độ bảo mất cho các request và response tại nay edge location thông qua **field-level encryption** và **lambda@edge**

#### Serving private content

AWS CloudFront cung cấp 2 phương pháp cho phép deliver private content cho 1 danh sách user nhất định:

1. signed URLs hoặc Signed cookies: temporary secure URLs or cookies that expire after a specified time help grant user access to a private file.
2. Origin Access Control (OAC): grant access vào private S3 bucket

##### Using sign URLS

Step to apply sign URLs for CloudFront distribution:

- Create a signer which can be AWS account or an IAM user that has permission to create signed URLs and signed cookies (recommended: a trusted key group with a public-private key pair).
- Configure your CloudFront distribution to use signed URLs as an additional layer of security. You can also provide custom JSON policy statement that dictate the access condition.
- Generate signed URLs using the private key

##### Origin Access Control

OAC tạo 1 secure channel nhằm serve file từ một private bucket đến người dùng thông qua CloudFront. OAC là phương pháp mới thay cho phương pháp OAI có những ưu điểm sau:

- Cho phép thức hiên **Server-Side Encryption with AWS KMS (SSE-KMS)**:
- Hỗ trợ nhiều HTTP method như PUT, POST, DELETE, và GET

Use Case: Use OAC to serve private S3 content (e.g., videos, documents) via CloudFront while blocking direct S3 access. It’s ideal for modern applications requiring encryption, dynamic operations, or cross-region setups.

#### Field-level encryption

với Field-level encryption, ta có thể encrypt sensitive data  (credit card numbers, SSNs, or other PII in web forms) cho mốt số field nhất định (tối đa 10 field) ngay tại edge location. Phương pháp này đảm bảo các thông tin nhạy cảm được bảo vệ trong suốt vòng đời của request từ client đến backend.

Limitation:

- thêm delay và chí phí từ quá trình mã hóa
- chỉ áp dụng cho method POST và PUT requests

#### Origin Failover

User tạo 1 group gồm nhiều origin và chọn các error status code (ví dụ 400, 501,... ) để trigger quá trình fall over đến origin hoạt động tốt.

#### CloudFront Functions  

JavaScript can be used to create what are called “lightweight” functions to monitor viewer requests and responses for customizations. CloudFront Functions must finish executing within sub-milliseconds. Use cases include  

- Modifying the HTTP request from the viewer: Return the modified request to CloudFront for processing. Headers, query strings, and URL paths can be modified.  
- Header manipulation: Insert, modify, or delete HTTP headers for the viewer request or response.  
- URL redirects: Redirect viewers to other pages based on information contained in the request, as shown in Figure 11-6.

#### Live streaming video

#### Lambda@Edge

`Lambda@Edge` is a feature of Amazon Web Services (AWS) that enables you to run serverless  functions in response to CloudFront requests to website data records.
`Lambda@Edge` functions execute at edge locations, providing fast and reliable performance for requests and queries.
Technical details of `Lambda@Edge` include the following:  

- `Lambda@Edge` functions are written in JavaScript using the Node.js runtime.  
- `Lambda@Edge` can be triggered in response to four different types of CloudFront events: viewer request, viewer response, origin request, and origin response.  
- `Lambda@Edge` is executed at the edge location, which provides faster response times.  
- `Lambda@Edge` executed in the context of a specific CloudFront distribution can access information about request and response details, such as  request headers and cookies

#### Pricing

CloudFront charges for data transfers out from its edge locations, along with HTTP or HTTPS requests. Pricing varies by usage type, geographical region, and feature selection.
