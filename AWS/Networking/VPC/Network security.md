#### Security group
A security group là service cho phép thiết lập rule kiểm xoát network outbound và inbound traffic cho 1 instance hay 1 Elastic network interface.

Để tạo một security group ta cần khai báo các thông tin sau:
- group name
- description
- VPC mà security group được đặt
- inbound và outbound rule

Inbound và outbound rule sẽ bao gồm các field sau:
- Source
- Protocol
- Port range

Inbound rule example:

| Source           | Protocol | Port range |
| ---------------- | -------- | ---------- |
| 198.51.100.10/32 | TCP      | 443        |
| 0.0.0.0/0        | TCP      | 443        |

Nếu 2 rule có cùng `protocol` và `port range` nhưng có `source` bị overlap lẫn nhau, rule có specific cao hơn (range bé hơn hoặc prefix length lớn hơn) sẽ được ưu tiên. Như trong table trên, rule đầu tiên có **prefix length** (`/32`) sẽ được apply.

Inbound và outbound trong security group cần tuân thủ các rule sau đây:
- You can specify allow rules, but not deny rules.
- When you first create a security group, it has no inbound rules. Therefore, no inbound traffic is allowed until you add inbound rules to the security group.
- When you first create a security group, it has an outbound rule that allows all outbound traffic from the resource. You can remove the rule and add outbound rules that allow specific outbound traffic only. If your security group has no outbound rules, no outbound traffic is allowed.
- When you associate multiple security groups with a resource, the rules from each security group are aggregated to form a single set of rules that are used to determine whether to allow access.

Mỗi 1 VPC sẽ có 1 default security group. Ta không thể xóa **default security group** này tuy nhiên vẫn có thể thêm hoặc thay đổi rule trong **default security group**.

default outbound rule:

| Source    | Protocol | Port range |
| --------- | -------- | ---------- |
| 0.0.0.0/0 | all      | all        |
| ::/0      | all      | all        |

Security group là stateful firewall, stateful ở đây là security group sẽ ghi nhớ các request đã allow trong 1 chiều để cho phép response của request pass theo chiều ngược lại. Ví dụ khi ta request 1 `npm` package từ `npm.org` để chạy ứng dụng, security group sẽ tự động cho phép  response data trả về từ `npm` registry.

#### Network Access Control Lists
NACL cũng cung cấp dịch vụ firewall giống như security group. Tuy nhiên NACL có nhiều điểm khác với security group: 
- NACL chỉ áp dụng cho subnet chứ không phải 1 instance như với security group. 
- NACL là stateless không tự động cho phép reply của request đã allow trước đó. 
- NACL bao gồm cả allow và deny rule
- NACL default outbound rule và inbound rule là ALLOW all traffic

NACL dùng Rule number để quyết định quyền ưu tiên của từng rule. Rule number càng nhỏ, ưu tiên càng cao. 

Default NACL inbound rule là ALLOW mọi inbound traffic. 
Ví dụ muốn giới hạn inbound traffic qua HTTP và port 80 chỉ ALLOW request từ address 198.51.100.10/32. Ta tạo 1 rule mới với rule number là 99 lên để override rule có number là 100.  

| Rule # | Protocol | Port range | Source           | Allow/Deny |
| ------ | -------- | ---------- | ---------------- | ---------- |
| 99     | HTTP     | 80         | 198.51.100.10/32 | ALLOW      |
| 100    | All      | All        | 0.0.0.0/0        | ALLOW      |
| 101    | All      | All        | ::/0             | ALLOW      |
| *      | All      | All        | 0.0.0.0/0        | DENY       |
| *      | All      | All        | ::/0             | DENY       |

Default outbound rule:

| Rule # | Protocol | Port range | Destination | Allow/Deny |
| ------ | -------- | ---------- | ----------- | ---------- |
| 100    | All      | All        | 0.0.0.0/0   | ALLOW      |
| *      | All      | All        | 0.0.0.0/0   | DENY       |
| *      | All      | All        | ::/0        | DENY       |

#### AWS Network Firewall

AWS Network Firewall cung cấp giải pháp tạo firewall bảo vệ toàn bộ VPC. 
Hỗ trợ tính năng traffic filter theo các tiêu chí như **domain**, **geographic IP**, hoặc **HTTP header**.
Các tính năng của AWS Network Firewall bao gồm:
- stateful firewall giống với security group.
- filter traffic cho HTTP/HTTPS tại Layer 7 và layer 3, 4 cho encrypted traffic sử dụng **TLS inspection**
- traffic filter theo domain name.
- traffic filter theo Geographic IP cho phép control traffic từ 1 geographic regions.
- active threat defense: tự động ngăn chặn các security threat và áp dụng các security policy managed bởi `AWS intelligent`.
#### AWS Web Application Firewall (WAF)
Khác với AWS Network Firewall dùng để bảo vệ VPC, AWS WAF cung cấp firewall service cho các application và service dùng HTTP/HTTPS.
Các service mà AWS WAF có thể áp dụng trực tiếp được bao gồm:
- Amazon CloudFront distribution
- Amazon API Gateway REST API
- Application Load Balancer 
- AWS AppSync
- Amazon Cognito user pool
- AWS App Runner
- AWS Verified Access instance
- AWS Amplify

WAF được đặt tại Application Layer (layer 7) cho phép WAF inspect content của HTTP/HTTPS request và response và tiến hành filter theo các tiêu chỉ sau: 
- **IP address** origin of the request    
- Country of origin of the request
- String match or regular expression (regex) match in a part of the request
- Size of a particular part of the request
- Detection of malicious SQL code or scripting
- Query strings
- HTTP methods
- Headers
- Cookies

với mỗi điều kiện filter, ta có thể đăng ký 1 action:  
- Allow the requests to go to the protected resource for processing and response.
- Block the requests.
- Count the requests.
- Run CAPTCHA or challenge checks against requests to verify human users and standard browser use.

Ngoài tính năng traffic filter, WAF còn được dùng để ngăn chặn các security threat như:
- SQL injection
- `DDOS` trên layer 7
- cross script injection


> [!note]
> AWS WAF là **stateless firewall.**
