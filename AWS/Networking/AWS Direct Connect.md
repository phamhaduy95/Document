#### Introduction 

**AWS Direct Connect** là giải pháp tạo physical connection thông qua cáp quang từ on-premise đến AWS mà không cần đi qua internet giúp đảm bảo bảo mật, tốc độ truyền tải và ổn định đường truyền.

**Direct Connect** yêu cầu on-premise gần với một **AWS Direct Connect Location** (AWS data center)

Khi hòa mạng on-premise với VPC thông qua **Direct Connect**, ta cần đảm bảo CIDR block của 2 bên không bị overlap.

Để hoàn tất việc kết nối giữa on-premise và VPC, ta cần tạo ở phía VPC một **Virtual Network Gateway** đóng vai trò là 1 endpoint cho phép các instance hay services trong VPC đến được on-premise network. Như các loại gateway khác, **Virtual Network Gateway** cần được associate với một route table routing traffic về gateway.

Bên phía on-premise, ta cần tạo một **Custom Network Gateway**.

AWS offers two types of Direct Connect connections:
- **Dedicated Connection**: A physical Ethernet port dedicated to your AWS account.
    - Capacities: 1 Gbps, 10 Gbps, 100 Gbps.
    - Suitable for high-bandwidth needs or full control.
- **Hosted Connection**: Provided by an AWS Direct Connect Partner, with shared capacity.
    - Capacities: 50 Mbps to 10 Gbps.
    - More flexible for smaller-scale needs or quick setup.

#### Pricing
AWS tính chi phí sử dụng Direct Connect theo các thông số sau:
- **Port-Hour Charges**: Based on connection type and speed
- **Data Transfer**: Outbound data from AWS.
