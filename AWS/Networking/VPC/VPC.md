#### Introduction
VPC provide network to contain cluster of EC2 instance.
You can establish connection from one VPC to different one, or public internet or private intranet 
You divide the primary VPC CIDR block into subnets.
VPC is exclusive to one AWS region. you can not access it outside from its original creation place
#### CIDR block
CIDR block mô ta dãi địa chỉ IP (IP address range) được cấu thành bởi 2 phần (segments) starting address và prefix length được chia cắt bởi / delimiter:
Ví dụ trong CIDR block 172.16.0.0/16 ta sẽ có:
- **172.16.0.0** là starting address của dãi mạng. 
- **/16** là **prefix length**: tổng số lượng address trong dải mạng. Ví dụ với **/16**  total address được tính 2^(32-16) = 65,536. Khi đó ta có thể tính last address 172.16.255.255

Với CIDR block là `172.16.0.0/28` sẽ bao gồm  256 address trong khoảng range từ `172.16.0.0` đến `172.16.0.255`.

CIDR block được AWS dùng để mô tả dãi mạng cho VPC và subnets

Để dễ dàng phân biệt giữa IP address dùng nội bộ và public internet address, ta thường sử dụng RFC 1918 rule để tạo CIDR cho các private block  trong VPC:
- `10.0.0.0–10.255.255.255` (`10.0.0.0/8`)  
- `172.16.0.0–172.31.255.255` (`172.16.0.0/12`)  
- `192.168.0.0–192.168.255.255 `(`192.168.0.0/16`)

Lưu ý CIDR hỗ trợ cả IdV4 và idV6
#### Subnets
Subnet là 1 dãi IP address con trong 1 VPC. 
1 VPC có thể chứa nhiều subnet. 
Subnet thường được dùng làm nơi để đặt các AWS resource như EC2, RDS.
Một subnet chỉ được đặt trong 1 AZ duy nhất.

Khi khai bao subnet ta cần cung cấp các thông tin sau:
- **Name:** subnet name
- **CIDR Block:** `10.0.3.0/24`
- **Associated Route Table:** (optional)  subnet sẽ associate với main route table trong trường hợp user không cung cấp route table cụ thể nào.
- **Availability Zone:**  Available Zone sẻ chứa đựng subnet.
 
 Trong 1 CIDR block của subnet. 4 địa chỉ đầu tiên và địa chỉ cuối cùng của dãy mạng sẽ được reserved cho mục đích network management trong VPC.
#### Elastic Network Interfaces
ENI là virtual network card instance cho phép instance đó giao tiếp các resource khác trong mạng VPC.
Mỗi 1 instance khi được khởi tạo sẽ được kèm theo 1 ENI mặc định gọi là **primary network interface**. Ngoài ra user có thể tạo 1 **independent** ENI trong 1 subnet và gắn **secondary network interface** đó vào 1 instance đã tồn tại trong VPC. secondary ENI không nhất thiết phải nằm trong 1 subnet mà có thể tồn tại tại 1 subnet khác trong 1 region 

Mỗi 1 ENI mặc định sẽ được cung cấp 1 **primary private address**, address remove hoặc thay đổi trừ khi ta terminate instance có ENI đó.  
User còn có thể gắn thêm 1 **private address** khác với điều kiện là address mới phải cùng nằm trong 1 subnet với **primary address**.
#### Internet gateway
Internet gateway cho phép instance sở hữu 1 public IP address để kết nối với internet. Mỗi 1 VPC sẽ được kèm theo 1 internet gateway mặc định. 
#### Route table
Route table là một bộ rules set bao gồm các route quy định nới đến (`target`) của các traffic đến 1 địa chỉ (`destination`) trong mạng VPC. Để sử dụng route table, ta sẽ phải [associate 1 subnet với route table đó](https://www.youtube.com/watch?v=H0v39jFWf7E) nhằm thiết lập routing logic cho các instance trong subnet. 

| Destination   | Target       |
| ------------- | ------------ |
| `10.0.0.0/16` | `local`      |
| `0.0.0.0/0`   | `igw-123456` |
Ví dụ route table trên: có 2 route 
Route table sẽ yêu tiên route specific hơn để điều hướng

các target phổ biến trong route table: 
- **Internet Gateway:** For traffic destined for the public internet.
- **Virtual Private Gateway:** For traffic that needs to travel over a VPN connection to an on-premises network.
- **NAT Gateway:** For traffic from a private subnet that needs to access the internet.
- **Network Interface:** For traffic that is directed to another resource, such as a virtual appliance, within the same network.
- **VPC Peering Connection:** For traffic meant for a different virtual private cloud (VPC).
- **Local:** A default route that allows communication within the VPC itself.

Mỗi VPC sẽ có 1 default route table gọi là **main route table**. Tất cả các subnet chưa explicit associate với bất kỳ route table nào sẽ được associate với **main route table**. Main route table có định dạng sau đây

| Destination   | Target       |
| ------------- | ------------ |
| `10.0.0.0/16` | `local`      |
| `0.0.0.0/0`   | `igw-123456` |

Note: CIDR blocks for IPv4 and IPv6 are treated separately. For example, a route with a destination CIDR of `0.0.0.0/0` does not automatically include all IPv6 addresses
#### Default VPC
với mỗi account, AWS sẽ tự động tạo 1 default VPC trên mỗi region với cấu hình sau đây: 
- IPv4 CIDR block (`172.31.0.0/16`) total 65,536 address
- 1 default subnet  kích thước `/20` ỡ mỗi Availability Zone trong region đó. 
- 1 default internet gateway.
- 1 main route table với default route điều hướng mọi traffic từ (`0.0.0.0/0`) đến internet gateway.
- 1 default security group.
- 1 default network access control list (ACL).
