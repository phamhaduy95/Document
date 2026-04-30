#### Introduction
AWS Route 53 cung cấp dịch vụ sau:
- Domain registration  
- DNS management
- Availability monitoring (health checks)  
- Traffic management (routing policies)

Route 53 đóng vai trò quan trọng trong multiple-AZ hoặc multiple-region deployment. Nhiều service sử dụng route 53 update DNS record để thực hiện fall over về region hoặc AZ khác
#### Domain registration 
AWS 53 cung cấp dịch vụ cho thuê domain name và kết nối domain name đó với infra trong AWS. 

Ta có thể chuyển một domain đã đăng ký ở 1 domain registrar khác bằng việc unlock domain name transfer trong domain registrar admin và request authentication code. Sau đó ta cung cấp code cho AWS 53 để có đủ quyền

Nếu bạn không muốn leave domain registrar nhưng vẫn muốn sử dụng AWS 53 quản lý DNS configuration. Ta có thể lấy địa chỉ của name server address từ AWS 53 và tạo 1 NS record set vào trong zone files tại domain registrar.
#### DNS management
AWS 53 cho phép user 
- tạo 1 private hoặc public hosted zone
- Cập nhật các DNS record trong zone file

Quá trình update DNS record (`DNS propagation`) cần vài tiếng đồng hồ để hoàn thành cập nhật.
#### Availability Monitoring
AWS Route 53 cho phép tạo 1 health check function để thu thập tính trạng hoạt động của các resource quản lý bởi AWS 5.

Ta có thể forward kết quả health check cho Amazon CloudWatch alarms để gửi notification.
#### Routing Policies
Route 53 routing policies provide this kind of functionality at the domain level that can  
be applied globally across all AWS regions
- **Simple routing policy**:  kết nối domain name với 1 resource duy nhất
- **Weighted routing policy**: routing traffic tùy theo destination weight. Ví dụ 15% traffic về resource 1, 85% còn lại về resource 2 
- **Failover routing policy** – Use when you want to configure active-passive failover.
- **Latency routing policy** – Use when you have resources in multiple AWS Regions and you want to route traffic to the Region that provides the best latency. 
- **Geolocation routing policy** – Use when you want to route traffic based on the location of your users. 
- **Geo-proximity routing policy** – Use when you want to route traffic based on the location of your resources and, optionally, shift traffic from resources in one location to resources in another location.
- **IP-based routing policy** – Use when you want to route traffic based on the location of your users, and have the IP addresses that the traffic originates from.
- **Multi-value answer routing policy** – provide a simple form of **DNS-based load balancer** by returning up to eight healthy records in response to a DNS query. It's a way to use DNS as a built-in failover and load-balancing mechanism without relying on a traditional load balancer. 
#### Alias Record
An **alias record** is a special type of Amazon Route 53 record which help routing to other AWS resources. It is not a standard DNS record type and exists only within Route 53.

Some AWS resource we can create alias for:
- **API gateway API request**
- **VPC interface endpoint
- **CloudFront distribution**
- **Elastic Beanstalk environment**  
- **ELB load balancer** 
- **Global accelerator**
- **Another Route 53 record in the same hosted zone**
- **S3 bucket configured as a static website**

#### Domain and DNS
DNS  for mapping human-readable domain names (like example.com) to one or more machine-readable IP addresses (like `93.184.216.34`).

Namespace
set các domain name đã được sử dụng. 
Namespace phổ biến nhất IN (internet)
Do số lượng domain name trong 1 namespace thường rất lớn, nên các domain thường được quản lý theo kiến trúc phân tầng (hierarchy-structure).

Ở tầng thấp nhất là DNS zone. User thường quản lý các domain name thuộc sở hữu của mình thông qua DNS zone.

![[Domain Namespace Heirarchy.png]]
##### Name server
A name server is a specialized server that translates human-readable domain names into associated IP address.
##### Domain vs Domain name
**domain** đại diện cho server, compute resource.
**domain name** là readable name đại diện cho domain đó.
##### Domain name layers
domain name được cấu thành bởi nhiều thành phần. 
Ví dụ ta có domain name sau `admin.example.com`
`.com` là top-level domain (TLD) 
`example` là second-level domain (SLD)
`admin` là  subdomain.
##### Zones and Zone File
A DNS zone is a specific portion of the Domain Name System (DNS) that is managed by a single authority. It is like a section of a big map, where each section is controlled separately to make management easier.

A DNS zone is a segment of the DNS namespace, often a single domain like [example.com](https://example.com/), that contains DNS records to map domain names to IP addresses and other information
A zone file is a text file  that describes the way resources within the zone should be mapped to DNS addresses within the domain.
The file consists of several DNS records containing the data fields:
- Name : The domain or subdomain name being defined
- TTL: The time to live before the record expires
- Record Class: The namespace for this record—usually IN (Internet)
- Record Type: The record type defined by this record

Key Records:
- **Start of Authority (SOA) record:** A mandatory record at the beginning of the file that specifies the primary authoritative name server for the zone and other vital information, according to Cloudflare. 
- **Name Server (NS) records:** Define the name servers that hold the DNS records for the domain. **A record:** Maps a hostname to an IPv4 address. 

