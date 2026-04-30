#### Introduction

Aurora là 1 database engine được phát triển bới AWS với nhiều ưu điểm vượt trội so với RDS thông thường:

- **high availability** - tạo 6 storage redundancy trên 3 AZ khác nhau.
- **flexible scalability** - tự động scale storage tùy theo nhu cầu. Cho phép tạo tối đa 15 read replica để tăng hiệu năng đọc.
- **performance** - x5 throughput so với MySQL và x3 so với Postgree.
- **high compatibility** - có độ tương tính cao với Postgree và MySQL dễ dàng thực hiện data migration, data export/import giữa Aurora và 2 engine.

Điểm khác biệt lớn nhất giữa Aurora và RDS truyền thống là phần storage của Aurora được tách biệt và quản lý độc lập với compute instance. Với RDS mỗi compute instance sẽ kèm theo 1 block storage riêng (EBS) nhằm lưu trữ dữ liệu database.

Note: Nên mô tả thêm về shared volume trên Aurora.

#### Scalability for Read Operation

Aurora hỗ trợ horizontal scaling bằng việc tạo 1 cluster gồm các read replica (up to 15) trên một region.

Aurora áp dụng single-master architect để scale cluster. Cluster luôn bao gồm một master instance đảm nhận cả read và write và các read replica chỉ hỗ trợ read.

AWS sẽ sync về các read replica khi có data trên master replica được thay đổi. Tuy nhiên khác với RDS, do tất cả các instance trong Aurora điều dùng chung 1 volume, việc data synchronization giữa primary với các read replica gần như là ngay lập tức.

#### High availability

Aurora tạo 6 copy storage và đặt 6 copy này trên 3 AZ khác nhau. Khi gặp sự cố, Aurora sẽ tự failover về 1 trong copy storage để đảm bảo database service không bị gián đoạn.

Ngoài ra ta còn có **Aurora Global Database** hỗ trợ tạo backup trên nhiều regions khác nhau (up to 5) và sẽ promote 1 secondary cluster khi có sự cố.

#### Backup and Recovery

Quá trình data backup diễn ra tự động, incremental trên các secondary replica nên không ảnh hưởng đến database performance như đối với RDS.

Aurora hỗ trợ automatic và manual backup và point-in-time backup giống như RDS
#### Deployment mode

Aurora cung cấp 3 phương án deployment:

- **Aura provisioned**: giống với RDS, user chủ động chọn instance type, số lượng read replica, multiple AZ deployment. Aurora sẽ cung cấp serverless storage. Phù hợp với ứng dụng có steady và predictable workload.
- **Aurora Serverless:** tự động gia tăng compute instance và storage size theo demand. Phạm vi scaling sẽ nằm trong giới hạn maximum và minimum **Aurora Capacity Units** (ACU) được cung cấp.  Phù hợp cho các ứng dụng có variable workloads (workload biến động thường xuyên) và các ứng dụng **Multi-tenant** (tạo cluster riêng cho từng tenant).
- **Aurora DSQL**:  automatic ==limitless== horizontal scaling và global distribution. Phù hợp với ứng dụng có quy mô toàn cầu và đòi hỏi scaling cao.

#### Additional Features

##### Parallel Query (Aurora MySQL only)

A single query can be distributed across all the available CPUs in the storage layer to greatly  speed up analytical queries. More than 200 SQL functions, equijoins, and projections can run in parallel format.

#### Global Database

**Aurora Global Database** cho phép tạo thêm 10 read-only clusters ở nhiều region khác nhau từ 1 primary cluster. Trong trường primary cluster bị fail, **Aurora Global Database** sẽ promote 1 secondary cluster gần nhất làm primary cluster. 

Khi dữ liệu tại primary cluster được cấp nhật, AWS sẽ nhanh chóng sync cấp nhật về các secondary clusters để đảm bảo data consistency cho toàn hệ thống.

Aurora Global Database có các ưu điểm sau:

- **Globally high availability** - Tạo nhiều redundancy trên region khác nhau cho phép automatic failover về các secondary cluster khi có sự cố.
- **Low-latency read operations** -  secondary cluster sẽ serve các read request trong region chứa secondary cluster đó giúp giảm thiểu latency.
- **Disaster recovery** - khi có hiện tượng outrage trên primary cluster, AWS sẽ promote 1 secondary clusters gần đó thành primary cluster. Quá trình promotion diễn ra tối đa 1 min.

##### Zero-ETL Integration

Aurora offers zero-ETL integration with services like Amazon Redshift, Amazon OpenSearch, or AWS Glue, enabling direct analytics on transactional data without ETL pipelines.
