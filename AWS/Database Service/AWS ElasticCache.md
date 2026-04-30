#### Introduction

Amazon ElastiCache là giải pháp fully managed, in-memory data store và caching service
Các ứng dụng chính

- **Caching frequently accessed data:** cho database access và application server.
- **Real-time session store:** Storing user session data for personalized experiences in applications like gaming, e-commerce, and social media.
- **Real-time analytics and leaderboards:** Facilitating fast data processing and updates for dynamic content.

AWS ElasticCache support hai in-memory database engine sau: **Memcached** và **Valkey** (**Redis Folk**)

#### AWS Elastic for Memcached

**Memcached** là open source in-memory storage với các điểm mạnh sau đây:

- high performance với multi-threading với throughput và latency tốt hơn so với **Redis**
- light-weight và simple to use.

mặc dù vậy, **Memcached** vẫn có các nhược điểm sau:

- Chỉ hỗ trợ simple key-value store với data type là string.
- Không persist data như `redis`

**Memcached** thường được sử dụng cho các use case sau đây:

- caching cho SQL database cải thiện đáng kể throughput và access latency.
- session store lưu các user session data.

##### Scalability
Memcached supports horizontal scaling
ElastiCache for Memcached hỗ trợ scaling bằng việc tạo 1 cluster bao gồm 1 hoặc nhiều EC2 instances được gọi là node (tối đa 40 node). Cached data sẽ được phân bố đồng đều tại mỗi node (partition).
Các node trong 1 **Memcached Cluster** có thể nằm trên 1 AZ duy nhất hay nhiều AZ khác nhau trên cùng 1 region để đảm bảo tính High Availability.

#### AWS Elastic for Valkey (Redis folk)

Valkey là in-memory database được folk từ Redis nên compatible cao với redis.
AWS Elastic for Valkey là persistent storage
Các ứng dụng của Valkey:

- media streaming: in-memory data store to power live streaming use case
- real-time analytics:
- Geospatial:  data structures, and operators to manage real-time geospatial data at scale and speed
- SQL query caching

##### Auto Scaling

quản lý theo cluster nodes
ElastiCache for Redis scales through the addition of shards, which is a grouping of one to six related nodes. AWS Elastic for Valkey cache data can be partitioned up to 500 shards.

Each multiple-node shard has one read–write primary node and one to five replica nodes

ElastiCache for Redis autoscaling allows you to increase or decrease the desired shards or replicas automatically

##### Backup and recovery

ElastiCache for Redis supports automatic and manual backups to S3. với maximum backup retention limit is 35 days.

##### High Availability

AWS Elastic for Valkey has automatic recovery from cache node failures.  Multi-AZ deployment is supported for AWS Elastic for Valkey cluster nodes.

Global datastore is fast and secure cross-region replication: 2 cluster on 2 separated regions.

![[ElasticCache Global Data Store.png]]

#### Security

AWS Elastic for Valkey supports encryption in transit and encryption at rest, with authentication for HIPAA-compliant workloads.  
hỗ trợ encryption data in transit và data at rest
isolate network với VPC
IAM để manage access control


redis support nhiều data structure phức tạp cho nhiều nhu cầu khác nhau như hash map, 
redis là persistent store.
redis dùng master-replica replication cho phép tạo nhiều read replica tăng read operation và HA tự động failover về 1 replica 
redis có transaction
