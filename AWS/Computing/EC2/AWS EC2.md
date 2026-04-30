#### Introduction

AWS EC2 cung cấp dịch vụ cloud computing

Các configuration cho một EC2:

- **instance type**: vCPU, RAM, bandwidth, storage.
- **AMI** (**Amazon Machine Image**): golden image để khởi tạo VM.
- **security group** : firewall kiểm xoát inbound và outbound
- **User data** - custom script được kích hoạt mỗi khi instance được khởi động.
- **SSH key pair**: dùng để log in vào EC2 instance đó từ xa.
- **placement group**: cluster, spread or partition
- **tenancy**: shared, dedicated tenancy or dedicated host
- **subnet**: địa chỉ subnet mà instance sẽ được launch vào.

#### EC2 Amazon Machine Image (AMI)

AWS dùng AMI như 1 template để  khởi tạo instance, AIM chứa các thông số quan trọng như:

- **Boot volume**: The root boot volume for an EC2 instance can be either an EBS boot volume created from a snapshot or a local instance storage volume copied from an Amazon S3 bucket
- **Launch permissions**: danh sách các user được phép truy cập và quản lý EC2 instance.
- **Volumes to attach**: data volumes attached to the EC2 instance at launched. Thường là EBS.
- **Operating system**: Linux, Windows macOS
- **Root device storage**: Amazon EBS hoặc EC2 instance storage volume.

User có thể sử dụng AMI từ các nguồn sau đây:

- AWS Marketplace: danh sách đa dạng các AIM. Một số AIM còn có cài đặt sẵn third-party software.
- Custom AMI:  AWS cho phép user tự tạo 1 AIM từ 1 snapshot của 1 EC2 instance.

**EC2 Image builder**
Amazon EC2 Image Builder helps organizations create, manage, and maintain customized AMIs. With EC2 Image Builder, you can automate the process of building, testing, and distributing AMIs. Here are some key features of EC2 Image Builder:  

- **AMI creation:** EC2 Image Builder provides prebuilt image pipelines that enable you to quickly create custom AMIs without having to write any code.  
- **Automated testing**: EC2 Image Builder includes built-in testing capabilities that allow a variety of supplied tests and custom tests to validate the functionality and security of your AMIs.  
- Image distribution: EC2 Image Builder integrates with AWS IAM and Amazon S3 for secure distribution of AMIs to designated administrators and AWS accounts.

#### Logging into EC2 instance

Ta có thể access vào 1 EC2 instance thông qua nhiều cách:
session manager: log vào EC2 instance thông qua AWS console trên browser.
SSH channel:

Cách ta logging và EC2 instance phụ thuộc vào AIM type mà ta

#### EC2 placement group

Ta có thể config EC2 instance được sắp xếp theo các placement strategies như sau:
**Cluster**: đặt tất cả instances trong 1 physical hardware tại 1 Availability Zone, cho phép giảm thiểu đáng kể network latency và tăng hiệu năng. Tuy nhiên hardware failure dẫn đến toàn bộ cluster bị gián đoạn.
**Partition**: chia thành các nhóm nhỏ hơn gọi là Partition. Mỗi partition sẽ được đặt riêng ở 1 hardware riêng biệt. Best for Large-scale distributed and replicated workloads like Hadoop, Kafka, and Cassandra.
**Spread**: đặt rải rác các instance tại nhiều hardware khác nhau. (default)

#### EC2 Tenancy

EC2 Tenancy quyết định việc ta có share physical hardware với customer khác hay không.

- **Shared tenancy (Default):** The most common and cost-effective option, where your instance runs on shared hardware with instances from other AWS customers. Most EC2 instances use this tenancy by default
- **Dedicated Instances:** Your instances run on hardware dedicated to a single AWS account, ensuring physical isolation from other accounts. However, multiple Dedicated Instances within the same account might still share hardware, and you do not have control over which host your instances are placed on.
- **Dedicated Hosts:** A physical server with EC2 instance capacity entirely dedicated to your use.

Lưu ý: 
Ta chỉ có thể start hoặc stop một EBS


