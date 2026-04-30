#### Introduction

RDS Proxy quản lý **connection pool** giữa client đến database. khi có connection request, RDS Proxy thay vì thực hiện cycle khởi tạo và loại bỏ connection, RDS proxy sẽ cached các connection lại trong connection pool và reuse chúng cho các request mới .

RDS Proxy is a **full-managed** and **serverless** service.

Ưu điểm của RDS proxy:

- **high availability:**  hỗ trợ tự động failover (hiệu quả hơn so với DNS service) cho multiple AZ deployments
- **scalability**:  đóng vai trò như load balancer routing request cho một cluster gồm RDS instances.
- **security**: hỗ trợ SSL/TLS encryption cho data in transit, enforce IAM authorization cho DB access.
- **performance**: tối ưu trong việc quản lý connection bằng việc cache và reuse connection tránh việc khởi tạo và tear down connection quá nhiều lần.

#### Pricing

regular RDS: tính theo vCPU của instance
aurora: tính theo Aurora Compute Unit (ACU)

#### Use Case

RDS Proxy solves the use case of many small processes starting up and shutting down, creating new DB connections from scratch. For example 1000 separate Lambda function instances spin up and try to open 1000 separate connections to the database, and every 15 minutes they do this again, over and over. RDS proxy will offload a lot of that load onto itself so it doesn't impact the DB.
