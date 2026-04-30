#### Introduction

CloudTrail có nhiệm vụ log các event.

#### CloudTrail Event

event trong AWS CloudTrail là record  ghi lại một hoạt động của một principal (user hoặc AWS resource) trên một service và resource nhất định. Các record này
AWS CloudTrail track event trên mọi AWS platform từ AWS Web Console, code từ AWS SDK hay command line của AWS CLI.
AWS CloudTrail có thể chia ra làm
Action API:
Non Action API
Dựa vào mục đích sử dụng, event còn có thể chia thành 4 loại sau đây:

- management events: các event của các tác vụ quản lý các AWS service như khởi tạo, thay đổi cấu hình, đăng ký các dịch vụ hoặc xóa resource ( khởi tạo 1 EC2 instance, configure S3 bucket, ... ).
- data events:

#### CloudTrail storage

CloudTrail lưu các log event ở 3 nơi chính:

- **event history**: nơi lưu trữ mặc định và miễn phí của CloudTrail. Tuy nhiên chỉ lưu trữ các event thuộc management event type trong 90 ngày gần nhất và bị giới hạn cho 1 AWS account và AWS region.
- **trail**: event log sẽ được lưu tại S3 dưới dạng các file log theo định dạng JSON. User phải trả phí cho dịch vụ S3 để lưu giữ các log file tuy nhiên giới hạn 90 ngày của event history bị gỡ bỏ và user có quyền kiểm xoát  event nào sẽ được log vào trail.
- **CloudTrail lake**: dùng columnar database để lưu event log, hỗ trợ cú pháp SQL để query các event. Ngoài ra **CloudTrail Lake** còn có tính năng cho phép user tạo 1 custom event qua API **PutAuditEvents**.

#### Trail log file

**Trail log file** được lưu trong S3 dưới định dạng JSON.
Tên file của log trail log file:
**AccountID_CloudTrail_RegionName_YYYYMMDDTHHmmZ_UniqueString.FileNameFormat**

event được log trong trail log file có định dạng như sau:

- `eventTime`: thời gian event được log
- `userIdentity`: principal thực hiện trigger event
- `eventSource`: global endpoint của resource (e.g., `ec2.amazonaws.com`).
- `eventName`: The name of the API operation (e.g., `RunInstances`)
- `awsRegion`: `us-east-1`
- `sourceIPaddress` : private IP address của request

Trail chỉ cung cấp dịch vụ lưu trữ và không kèm theo tính năng truy vấn event log.

#### CloudTrail Lake

Các điểm mạnh của CloudTrail Lake:

- Mặc dù vẫn phải cài đặt tại một region cụ thể như event history và trail, trail lake hổ trợ aggregate event log từ nhiều region account khác nhau.
- Có serverless database chuyên dụng hỗ trợ query event chuyên sâu và phức tạp.
- Cho phép người dùng tạo các custom event ngoài các basic event trong AWS system.

#### security practice

Ngoại trừ **Event History** và **CloudTrail Lake** đã mặc định thực hiện encryption và đảm bảo immutability cho các event log, user cần enable encryption và validation cho **trail log file** lưu trong S3 để đảm bảo tính bảo mật.

Ta có thể năng cao tính bảo mật cho CloudTrail:

- enable KMS để encrypt log file lưu trong S3.
- bật chế độ
validate log files:

Note that it can take up to 15 minutes between the time  
an event occurs and when the CloudTrail creates the log file containing the event

Modern modular design can help create workloads constructed with many loosely  
coupled services integrated using a common set of APIs, creating a functional workload with micro-service architectures.

 To log and audit access to your AWS resources



