#### Introduction

**AWS Global Accelerator** giúp cải thiện availability và tốc độ truy cập của application trên quy mô toàn cầu. Thay vì sử dụng internet thông thường, **AWS Global Accelerator** tận dụng **AWS Private Network** một mạng private mạnh mẽ kết nối tới hàng trăm **edge location** trên toàn thế giới giúp duy trì latency thấp cho các end user. 

**AWS Global Accelerator** còn thực hiện health check thường xuyên để đảm bảo failover khi có sự cố.

AWS sẽ cung cấp hai **static Anycast IP Address** giúp end user kết nối được tới application. Mỗi IP address có thể map tới nhiều endpoint khác nhau của application ở nhiều region. AWS Global Accelerator sẽ monitor health và thực hiện failover để đảm bảo high availability. **AWS Global Accelerator** đảm bảo Anycast IP address sẽ không bị thay đổi khi quá trinh failover diễn ra.

**AWS Global Accelerator** có thể tạo listener theo port để forward về các origin sau: 
- Application Load Balancer 
- Network Load Balancer
- EC2 instance
- Elastic IPs
#### Applications

**AWS Global Accelerator** có các ứng dụng phổ biến sau:
- Global apps cần low-latency access (e.g., e-commerce, streaming).
- Multi-region deployments for better failover.
- As an alternative/complement to Amazon CloudFront (Global Accelerator focuses on TCP/UDP traffic to origins, while CloudFront is HTTP/HTTPS-centric).

Khác với CloudFront, AWS Global Accelerator không support caching nên không tối ưu cho các ứng dụng web thông thường phù hợp với các ứng dụng trực tuyến như game online, chat application.
