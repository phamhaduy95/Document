#### Introduction

**EC2 Auto Scaling** là dịch vụ cung cấp giải pháp horizontal scaling giúp tăng giảm số lượng instance để đáp ứng tải trọng khác nhau.
**EC2 Auto Scaling** sẽ thực hiện quá trình scaling  trên 1 **Auto Scaling Group**

#### Launch template

User cài đặt thông số hoạt động của **EC2 Auto Scaling** thông qua **Launch Template**.
Các configuration cho Launch template bao gồm:  

1. Các configuration cơ bản của EC2 instance như instance type, user data, AMI, EBS volume, log in pair key, VPC, ...
2. **Health check**: HTTP endpoint cho phép thu thập tình trạng hoạt động của các instance trong **Auto Scaling Group**
3. **Auto scaling policy**: user-defined logic control quá trình scaling in/out cho EC2 Auto Scaling.

Launch template áp dụng cơ chế versioning cho phép user lưu lại các phiên bản cũ hơn trong template history.

EC2 Auto Scaling dựa vào các metric thu thập bởi AWS CloudWatch để tiến hành scaling cho phù hợp. By default, metric **CPU utilization metric average** sẽ được dùng để làm threshold value

#### Enable Health Check Instance

Trong launch template, user có thể khai báo health check 1 hàm HTTP method cho nhiệm vụ kiểm tra
Ngoài hàm *health check* mặc định này, ta còn có thể dùng thêm hàm health check từ **Elastic Load Balancer** 's target group. Với sự kết hợp này, mỗi khi có một instance trong trạng thái không tốt, **ELB** sẽ có nhiệm vụ redirect request từ instance đó sang các instance khác trong khi **Auto Scaling** sẽ terminate instance và thay thế bởi instance mới.

#### Auto Scaling Policy

User có thể điều chỉnh cách thức hoạt động của Auto Scaling thông Scaling Policy. 1 Scaling Policy thường được cấu thành bởi hai phần.

Phần một mô tả phạm vi và số lượng EC2 instance mà **Auto Scaling** cần đảm bảo, phần này bao gồm các thông số chính sau:

- **Maximum size** - số instance tối đa mà Auto Scaling có thể *scale out*
- **Minimum size** -  lượng Instance tối thiểu mà Auto Scaling có thể *scale in*
- **Desired capacity** - số lượng instance lý tưởng mà Auto Scaling cần duy trì.

Ví dụ: Ta set bộ thông số sau cho **EC2 Auto Scaling** (`max: 10, min:1, desired value: 5`).
Auto Scaling sẽ tiến hành scale hệ thống sao cho số lượng instance match với desired value là 5. Khi user terminate 2 running instances, 2 instance mới sẽ được khởi tạo và thêm vào scaling group nhằm đảm bảo số lượng luôn về desired value.

Phần hai là Auto Scaling option, mô tả cách thức scaling Có các mode auto Scaling như sau:

- Simple Scaling Policies
- Step Scaling Policies
- Tracked Scaling Policies

##### Manual Scaling

Khi user thay đổi các giá trị **max**, **min** và **desired value** trong policy, Auto Scaling sẽ scale số lượng instance hoạt động để phù hợp vối thông số mới.

##### Simple Scaling Policies

user đặt một metric threshold như 80% (`CPUUtilization`)  Auto Scaling sẽ tiến hành scale out/in tương ứng để hệ thống hoạt đông dưới mức threshold trên.
User còn có thể set số lượng instance Auto Scaling tăng hoặc giảm thông qua `adjustment types`

- `ChangeInCapacity` - tăng giảm theo một số lượng cố định. Ví dụ ta set `ChangeInCapacity = 2`, khi Auto Scaling sẽ tăng số lượng các instance thêm 2.
- `ExactCapacity` - Auto Scaling duy trì 1 số lượng instance cố định.
- `PercentChangeInCapacity` - Tăng, giảm theo phân trăm số lượng instance hiện tại.

Auto Scaling sẽ có 1 khoảng thời gian *cooldown* *period* sau mỗi lần điều chỉnh.
Giá trị mặc định của thời gian *cooldown* là 300 giây.

##### Step Scaling Policies

Cho phép ta đặt nhiều khoảng giới hạn khác nhau với mỗi giới hạn sẽ có một adjustment type khác nhau.

khoảng giới hạn cần có thông số sau:
- A lower bound
- An upper bound  
- The adjustment type  
- The amount by which to increase the desired capacity

có cooldown period để tiến hành scaling.
##### Tracked Target Policies
User được phép lựu chọn vào 1 metric và target value làm một tiêu chuẩn để Auto Scaling thay đổi số lượng instance để đáp ứng.

Theo thông kế thực nghiệm, **Tracking Target Policy** và phương án hiệu quả nhất về giá cả cũng như hiệu năng. Chính vì thế policy này được dùng mặc định cho Auto Scaling Group.

**Scheduled Actions**

#### Health check Grace Period

The ELB performs health checks continuously on registered targets. The **grace period** prevents the Auto Scaling group from prematurely terminating newly launched instances that haven't had enough time to initialize and pass their first health checks

- **Instance Launch:** When an Auto Scaling group launches a new EC2 instance, the instance needs time to boot up, install software, and start its application server. This process can take several minutes.
- **ELB Health Checks:** The ELB starts running health checks on the new instance immediately once it's registered.
- **The Grace Period (The Role of the ASG Setting):** During the specified health check grace period, the **Auto Scaling group ignores the results** of the ELB's health checks when determining whether to terminate the instance.
- **After the Grace Period:** Once the grace period ends, the Auto Scaling group begins to act on the ELB health check results. If the instance is still failing health checks at this point, the Auto Scaling group will mark it as unhealthy and terminate/replace it.

#### Auto Scaling life cycle hooks
![[EC2 life cylce.png]]

Auto Scaling cho phép ta trigger event đến SNS topic cho các event sau đây
- An instance is launched
- An instance is terminated
- An instance fails to launch 
- An instance fails to terminate

```yaml
aws autoscaling put-lifecycle-hook \
  --lifecycle-hook-name ConfigureInstanceHook \
  --auto-scaling-group-name MyWebASG \
  --lifecycle-transition autoscaling:EC2_INSTANCE_LAUNCHING \
  --notification-target-arn arn:aws:sns:ap-southeast-1:123456789012:MyTopic \
  --role-arn arn:aws:iam::123456789012:role/AutoScalingNotificationRole \
  --heartbeat-timeout 300 \
  --default-result CONTINUE
```


từ SNS topic, ta có thể forward đến 1 lambda function để tiến hành process.