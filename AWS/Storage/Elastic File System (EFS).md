external scalable shareable file system 
elastic: scale tự động theo demand không cần phải manually thay đổi storage size như EBS.
shareable storage cho nhiều instances chứ không attach riêng với chỉ 1 instance như EBS.
availability cao do là full-managed serverless: AWS tự sao lưu backup ở nhiều AZ khác nhau. 

kết nối với EC2 instance thông qua Network File System (NFS) mounts.
kết nối vối on-premise server thông qua Direct Connect connections.

**Cost optimization**
pricing của EFS phụ thuộc vào lượng storage đang sử dụng và traffic request data.
giống với S3, Amazon EFS cung cấp 3 dòng storage chính: EFS Standard, EFS Infrequent Access, and EFS Archive.
Cho phép user tạo 1 policy quản lý life cycle của object như vối S3 và dòng intelligent tier tự động chuyển class cho các object.
**Performance**
Ngoài dòng standard, EBS có dòng max/IO serve được nhiều request từ nhiều instance cùng 1 lúc.


Choose Regional to store data redundantly across multiple availability zones. Choose One Zone to store data redundantly within a single availability zone. 
Amazon EFS can store and share data access for Amazon EC2 instances, containerized applications, and on-premises servers

back up with AWS Backup

Elastic: The EFS file system automatically scales as you add and remove files;  
you do not need to select an initial storage size

Performance: Two performance modes are available: General Purpose and  Max I/O. Max I/O is designed for thousands of instances that need access to  the same files at the same time

Lifecycle management: Amazon EFS Intelligent-Tiering automatically  
transitions files in and out of Standard-infrequent Access storage based on a  
defined number of days since last access.

Concurrent connections to an EFS file system can be made from multiple  subnets

