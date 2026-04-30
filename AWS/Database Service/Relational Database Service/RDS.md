#### Introduction

AWS RDS cung cấp infra lưu trữ cho relational database.
Bản chất của RDS là compute EC2 instance được gắn 1 EBS volume để lưu trữ dữ liệu. Tuy nhiên khác với EC2 instance thông thường, RDS là **fully managed** service nên được AWS tự động cấp nhật OS, DBRM và user không có thể truy cập trực tiếp vào RDS instance như qua SSH, remote control  
#### RDS types

khi lựa chọn RDS instance, ta cần quan tâm đến các thông số sau:

- **disk throughput**: tính theo Mb/s hoặc IOPS (**Input/Output Per Second**) IOPS càng cao, lên quan đến tốc độ đọc và chép data vào ổ đĩa.
- **storage size:** kích thước vùng nhờ ổ cứng SSD.
- **hardware của instance**: CPU/RAM của compute Instance, ảnh hưởng trực tiếp đến hiệu năng của DB engine.
- **network bandwidth**: Tốc độ đường truyền của network.

Cũng giống với EC2, RDS cung cấp các chủng loại compute instance sau:

- **Standard**: Dòng tiêu chuẩn cân bằng về hiệu năng CPU, Ram, storage và network bandwidth
- **Compute Optimized**: hiệu năng CPU cao.
- **Memory Optimized**: có disk storage lớn, tối ưu hóa trong việc lưu trự giữ liêu lớn.
- **Burstable performance**: giống burstable instance bên EC2, cho phép hoạt động theo một baseline. Phù hợp với các ứng dụng trong môi trường development hoặc test.

#### Storage type

**General-Purpose SSD (gp2)** cung cấp vùng nhớ từ 20GiB-65TeB (giá trị còn phụ thuộc DB engine). Với mỗi GiB, chỉ số IOPS sẽ được công thêm 3 với maximum là 16000 **IOPS**. Cho phép burstable quanh một baseline IOPS (ví dụ baseline của volume 1000GiB là 3000 **IOPS**).

**Provisioned IOPS SSD (io1)** cho phép user provision **IOPS** thêm cho RDS instance (max 256,000 **IOPS**).  AWS sẽ charge thêm chi phí cho mỗi IOPS được thêm vào. Dòng RDS này phù hợp với các ứng dụng đồi hỏi I/O intensive database workloads

**Provisioned IOPS Block Express storage (io2)**

User có thể thay đổi instance và storage type cho một RDS instance đang hoạt động. Tuy nhiên, quá trình thay đổi có downtime khá cao. Ta có thể tác vụ này trong maintain window.

#### High Scalability with Read Replica

AWS hỗ trợ tính năng horizontal scaling cho RDS bằng cách tạo một cluster gồm nhiều **read replica**. Một cluster thường bao gồm một instance là ***master replica***, thực hiện cùng một lúc hai tác vụ *read* và *write*, và các ***secondary read replica*** chỉ có nhiệm vụ *read*. Khi dữ liệu tại master replica được cập nhật, RDS sẽ sync data bất đồng bộ (**asynchronous**) cho các read replica còn lại trong cluster.

AWS sẽ cung cấp một read-only endpoint cho mỗi cluster được khởi tạo. AWS sẽ cân bằng tải và điều hướng read request đến từng read replica. 

AWS cho phép ta promote một read replica khác thành ***master replica***. Tuy nhiên cần lưu ý do quá trình sync data từ master đến các replica diễn ra không lập tức,  trong quá trình promotion, có thể xuất hiện trường hợp dữ liệu bị mất hoặc corrupted, Điều này có thể dẫn đến data trên master replica mới chưa được cập nhật đầy đủ.

Tùy thuộc vào DB engine được sử dụng, ta có giới hạn cho số lượng read replica sau:

- MySQL, Postgree, MariaDB: 15 replicas
- Oracle, Microsoft server: 5 replicas

>[!important]
>Các read replica có thể được đặt trong 1 single AZ, trên nhiều AZ hoặc nhiều regions khác nhau.

#### High Availability with Multiple AZ

Khi chế độ Multiple AZ được bật, AWS sẽ tạo 1 **standby replica** ở một AZ khác từ RDS instance gốc (**primary replica**). Khi có sự cố gián đoán diễn ra, AWS sẽ cấp nhật DNS record điều hướng về **standby replica** để duy trì hoạt động. Quá trình fall over diễn ra không quá 60s với 1 standby replica và 35s với 2 standby replica (chỉ áp dụng cho Postgree và MySQL).

Khác với read replica cluster, quá trình sync data của multiple AZ là synchronous nên data trên cả **primary replica** và **standby replica** thường có sự đồng nhất.

> [!note]
> **standby replica** sẽ không hoạt động và serve request cho đến khi  có sự cố và quá trình failover hoàn tất.

Khi Multiple AZ được enabled, AWS sẽ tạo 1 instance làm standby replica và tiến hành sao lưu data từ primary replica. Quá trình này có thể dẫn đến service disruption lớn.

#### RDS cluster (MySQL và Postgree)
2 standby replica
Provides a higher level of data consistency due to semi-synchronous replication.

Automatic failover to one of the readable standby instances. The process is much faster than a standard Multi-AZ deployment (typically under 35 seconds) because the standby is already a running instance.

Provides a higher level of data consistency due to semi-synchronous replication.

Traffic is split between a "writer endpoint" and a "reader endpoint".

A cluster with one primary (writer) DB instance and two readable standby instances, all in different Availability Zones (AZs).

#### Backup for recovery

RDS hỗ trợ tính năng backup data thông qua lấy volume snapshot của RDS instance và sao lưu backup đó trong S3 bucket.

Trong quá trình backup diễn ra, RDS instance được backup sẽ có hiện tượng gián đoạn trong vài giây hoặc vài phút. Để tránh ảnh hưởng nhiều đến service availability, ta nên set backup window vào thời điểm database ít sử dụng nhất, hoặc bật tính năng multiple AZ cho phép backup được thực hiện trên **standby replica**

**Point in time backup**: Tính năng point-in-time backup và restoration được tự động kích hoạt khi dùng tính năng automatic backup. Point-in-time backup cho phép ta restore về trạng thái của database tại 1 giây cụ thể.

Quy trình lấy point-in-time backup của RDS sẽ diễn ra như sau: RDS sẽ tạo 1 backup hoàn chỉnh từ database chính trong backup window. Sau đó, RDS sẽ capture các transaction logs để lưu các thay đổi theo từng giây.

> [!important]
> Khi ta restore từ 1 backup nào đó, AWS sẽ tạo 1 RDS instance hoàn toàn mới.

#### Security

Các biện pháp đảm bảo tiêu chi bảo mật cho RDS instance.
- Thực hiện data encryption thông qua dịch vụ KMS.
- Sử dụng (SSL/TLS) connections để encrypt data được truyền tải.
- Nên đặt các RDS instance trong các private subnet.
- attach security group giới hạn request đến RDS.

#### Pricing




Note:
- > A Read Replica of an Amazon RDS encrypted instance is also encrypted using the same key as the master instance when both are in the same region
- > If the master and Read Replica are in different regions, you encrypt using the encryption key for that region
- > You cannot encrypt an existing DB, you need to create a snapshot, copy it, encrypt the copy, then build an encrypted DB from the snapshot