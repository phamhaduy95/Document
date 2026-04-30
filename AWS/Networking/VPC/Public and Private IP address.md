Mỗi 1 instance khi được launch trong 1 VPC đều được cung cấp ngẫu nhiên 1 private address nằm trong CIDR block subnet. Ta có thể dùng address này để kết nối đến instance khác trong VPC.
#### Public IP address
Khi 1 instance được tạo trong 1 public subnet (subnet associate vối route table đến internet gateway). AWS sẽ tạo 1 public address cho instance này. Ta có thể turn off tính năng auto attach public Ip address này trong subnet configuration.

Mỗi một public Ipv4 address sẽ bị tính phí $0.005 mỗi giờ.

Một lưu ý quan trọng là public address này không cố định. AWS sẽ thu hồi address này khi instance bị stopped hoặc terminated. 

Khi ta launch 1 instance trong default subnet, instance đó sẽ được gắn một public address. Do default subnet được associate với main route table có default route trỏ về internet gateway. 

Lưu ý khi dùng **Application Load Balancer** để cân bằng tải cho 1 EC2 cluster ta nên tắt assign public IP address cho EC2 instance. Do ELB sẽ có public address riêng và mọi traffic phải qua ELB để đến các EC2 instance.
#### Elastic IP address
Khác với public ID address ở trên, Elastic IP address là IP tĩnh tức sẽ không bị AWS thu hồi khi instance bị stop hay terminated.
Elastic IP address không liên kết trực tiếp tới một instance nào mà sẽ được gắn vào một Elastic Network Interface (ENI) tự do. Khi muốn gắn Elastic IP address cho một EC2 instance, ta chỉ cần assign ENI này cho EC instance dưới danh nghĩa là secondary interface. 

**Lưu ý**:
Elastic IP address bị lock trong một region và không thể transfer qua một region khác. Tuy nhiên ta có thể thêm năm IP address do chính ta sở hữu trong một region. 

Muốn xóa Elastic IP address, ta phải xóa trước INE sử dụng IP address. 