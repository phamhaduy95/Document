#### EC2 pricing

AWS chỉ charge user theo thời gian EC2 instance được sử dụng.
AWS sẽ không tính phí cho instance trong trạng thái ngừng hoạt động (stopped)

Giá của mỗi instance sẽ phụ thuộc và các yếu tố sau:

- **instance type**: instance có hardware, có hiệu năng càng tốt và càng chuyên biệt thì giá càng cao.
- **AIM**: một số AIM có kèm theo OS và phần mềm có trả phí.
- **tenancy**: instance càng isolate, giá càng cao.

Ngoài ra, AWS còn có nhiều billing plan cho user lựa chọn tùy theo nhu cầu sử dụng.

- **on demand**: charge theo giờ hoặc giây (tối thiểu là 60s). Phù hợp cho nhu cầu ngắn hạn.
- **saving plans**: yêu cầu user commit trong một giới hạn sử dụng (usage per hour) trong khoảng thời gian dài (1->3 năm) đổi lại user nhận được discount (up to 72%). phù hợp cho các ứng dụng chạy liên tục và ổn định như web servers.
- **reserved plans:** giống với như saving plan, tuy nhiên bị giới hạn trong một region và instance type.
- **spot instances**:

#### EC2 Instance type

EC2 có catalog đa dạng chung loại EC2 tùy theo mục đính sử dụng.

- **General-purposed instances**: phù hợp với đa số ứng dụng phổ thông như web application. Ví dụ: `m8g`, `m7a`, `t3`,`t4`
- **Compute Optimized Instances**: sở hữu bộ CPU mạnh mẽ phù hợp cho các ứng dụng đòi hỏi như cầu tính toán lớn. Các dòng tiêu biểu bao gồm `c8g`, `c7i`
- **Memory Optimized instances**: tối ưu về lưu lượng và IO throughput cho RAM phù hợp với các ứng dụng in-memory databases, real-time data analyst.
- **Accelerated Computing instances**: có GPU mạch mẽ phù hợp cho các tác vụ parallel computing như image processing và train AI model. Các instance tiêu biểu gồm `p5`, `g6`

##### Burstable instances

 instance bị giới hạn trong một baseline (tính theo % CPU sử dụng). Mỗi khi baseline đó bị vượt qua,user sẽ bị trừ điểm credit theo từng dây và được cộng thêm credit cho mỗi giây hoạt động dưới baseline. Khi credit sử dụng hết, AWS sẽ throttle hiệu năng của EC2 xuống.
 **Burstable instance** phù hợp cho các mục đính testing, xây dưng dự án cá nhân.

##### Spotting instance

- _EC2 instance rebalance recommendation_ – Amazon EC2 emits an instance rebalance recommendation signal to notify you that a Spot Instance is at an elevated risk of interruption. This signal provides an opportunity to proactively rebalance your workloads across existing or new Spot Instances without having to wait for the two-minute Spot Instance interruption notice.

- _Spot Instance interruption_ – Amazon EC2 terminates, stops, or hibernates your Spot Instance when Amazon EC2 needs the capacity back. Amazon EC2 provides a Spot Instance interruption notice, which gives the instance a two-minute warning before it is interrupted.


- > You can specify whether Amazon EC2 should hibernate, stop, or terminate Spot Instances when they are interrupted. You can choose the interruption behavior that meets your needs. The default is to terminate Spot Instances when they are interrupted

