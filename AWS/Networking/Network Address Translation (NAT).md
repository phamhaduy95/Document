#### Introduction
NAT là công cụ cho phép EC2 instance trong một private subnet có quyền access internet theo một chiều duy nhất outbound.

AWS cung cấp 2 giải pháp chính:

- **NAT instance**: chạy trên một EC2 compute instance, yêu cầu nhiều manually configuration để hoạt động. 
- **NAT gateway**: serverless NAT có thể scale theo demand.

Ta nên triển khai NAT cho từng AZ. 
#### NAT Instance

Quá trình triển khai NAT instance cho 1 private subnet gồm các bước sau: 

1. launch NAT instance trong một public subnet.
2. tạo 1 Elastic IP address và gắn vào NAT instance.
3. tạo 1 route table và thêm routing rule với target là id của NAT instance. 
4. associate **private subnet** cần internet access vào route table vừa tạo.

Ưu điểm: 

- customization cao do bản chất vẫn là EC2 instance, cho phép user apply security group và áp dụng Auto Scaling. 
- phù hợp với ứng dụng có nhu cầu sử dụng thấp.

Nhược điểm:

-  user phải tự patch OS và software và thay đổi instance type để đáp ứng bandwidth.
#### NAT Gateway

fully managed và serverless solution tự động scale bandwidth theo nhu cầu sử dụng. 

Thao tác triển khai NAT Gateway đơn giản hơn rất nhiều so với NAT instance:
- Create a NAT Gateway in a public subnet with an Elastic IP.
- Update the private subnet’s route table to route 0.0.0.0/0 traffic to the NAT Gateway’s ID.

#### Use case
 NAT Gateways or Instances enable private subnet RDS instances (e.g., MySQL Multi-AZ clusters) to access the internet for updates or backups to S3 without exposing them publicly.