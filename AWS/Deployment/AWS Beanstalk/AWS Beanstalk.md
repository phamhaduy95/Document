# Introduction

AWS Beanstalk đơn giản hóa quá trình deploy một application lên production. Chỉ cần upload application code và một file cài đặt mô tả cấu hình infrastructure cho EC2 instance, networking hay storage, AWS sẽ tự động khởi tạo và quản lý các resource cần thiết để chay application.

Beanstalk có hỗ trợ deploy containerized application thông qua docker platform.  

Dựa vào các configuration trong file cài đặt, Beanstalk sẽ tự động thực hiện các tác vụ sau:

- Setup compute resource  như EC2 instance, security group.
- Cung cấp database sử dụng RDS hoặc `dynamoDB`.
- Configure **Auto Scaling** + **Load Balancer**.
- Tạo S3 bucket để lưu giữ source code và  **CloudWatch** logs.
- assign 1 domain name.
- cài đặt **platform** bao gồm run-time và framework cần thiết để chạy application như NodeJS, Python

#### Pricing

AWS Beanstalk là dịch vụ hoàn toàn miễn phí. AWS chỉ tính tiền cho các resource và service được khởi tạo thông qua beanstalk.

#### Beanstalk Configuration

Các configuration cho AWS beanstalk cần lưu ý:
##### Environment Configuration
1. **Environment Tier**
	- **web server**: web application (frontend or backend)
	- **worker service**: long-running processes support messaging qua AWS SQS queue.
2. **Platform** : Runtime và Language như Apache Tomcat, Python
3. **VPC**
4. **Environment Properties (Environment Variables)**: key-value mapping cho biến môi trường
##### Instances (EC2) Configuration
1. **Instance Type:** The size and capabilities of the EC2 instances (e.g., `t2.micro`, `c5.large`).
2. **EC2 Key Pair:** The key pair used for SSH access to the instances for debugging.
3. **Security Groups:** The EC2 security groups that control inbound and outbound traffic to the instances.
4. **Root Volume:** The type and size of the EBS storage volume attached to the instances (e.g., General Purpose SSD, Provisioned IOPS SSD).
5. **IAM Instance Profile:** The IAM role that grants permissions to the application running on the EC2 instances to interact with other AWS services (like S3 or DynamoDB)
##### Auto Scaling Group Configuration

##### Load Balancer
- **Load Balancer Type:** Application Load Balancer (ALB), Network Load Balancer (NLB), or Classic Load Balancer (CLB).
- **Listeners and Ports:** Configuration of listeners, ports (e.g., 80, 443), and protocols (HTTP/HTTPS/TCP).
- **SSL/TLS Certificates:** Management of server certificates for secure HTTPS connections.
- **Health Checks:** The path, protocol, and criteria the load balancer uses to determine if an instance is healthy.
- **Session Stickiness:** Enabling "sticky sessions" to route a user's requests to the same instance
##### Monitoring & Logging
- **Health Reporting:** Standard or Enhanced health reporting, which provides more detailed metrics and streams health information to CloudWatch Logs.
- **Log Streaming:** Streaming instance logs (tail logs) to Amazon S3 or CloudWatch Logs for debugging purposes
##### Deployment Mode
- **All at once**: AWS beanstalk sẽ deploy lên tất cả EC2 instance. Không recommend, do service sẽ bị gián đoạn cho đến khi quá trình deployment hoàn tất.
- **Blue/green**: AWS keep các instance chưa phiên bản code cũ. AWS sẽ tạo các instance mới để deploy code mới lên. Khi quá trình deploy hoàn thành, AWS sẽ redirect traffic về các instance mới và terminate instance cũ. 
- **Rolling**:
- **`RollingWithAdditionalBatch`**: launch 1 extra batch gồm nhiều instance và deploy code trước lên batch đó. Tiến hành rolling dần dần cho toàn bộ instance cũ.
- **Traffic Splitting (canary)**:"
- **Immutable**: AWS tạo 1 copy cho toàn bộ instance của AutoScaling Group và deploy code mới vào đó trong khi vẫn duy trì auto scaling group cũ.

> [!note] lưu ý
> Blue/Green deployment là phương pháp khuyến nghị để deploy phiên bản mà runtime không compatible ví dụ: phiên bản cũ python và phiên bản mới javascript 