#### Compute

**AWS Outposts**: A family of fully managed solutions that extends AWS infrastructure, services, APIs, and tools to your on-premises facilities. This offers a truly consistent hybrid experience, allowing you to run certain AWS services locally while connecting to the broader AWS Region.
    - **Outposts racks**: For data centers or on-premises facilities that need to run multiple services, such as Amazon Elastic Compute Cloud (EC2) instances and Amazon Simple Storage Service (S3).
    - **Outposts servers**: For edge locations with limited space or smaller capacity needs, like retail stores or branch offices.
- **Amazon Elastic Container Service (ECS) Anywhere and Amazon Elastic Kubernetes Service (EKS) Anywhere**: Provide consistent tooling and APIs for running containerized applications both in the AWS Cloud and on your own infrastructure.

---
#### Storage and data transfer

- **AWS Storage Gateway**: A hybrid cloud storage service that provides on-premises applications with low-latency access to virtually unlimited AWS cloud storage. It offers several gateway types:
    - **Amazon S3 File Gateway**: Store files as objects in Amazon S3.
    - **Amazon FSx File Gateway**: Provides low-latency, on-premises access to your Amazon FSx for Windows File Server data.
    - **Tape Gateway**: A virtual tape library (VTL) to help move backups and archives to the cloud.
- **AWS DataSync**: A data transfer service that makes it easy to automate and accelerate moving large amounts of data between on-premises storage systems and AWS storage services.
- **AWS Snow Family**: A collection of physical devices for transferring petabytes of data into and out of AWS, especially in environments with limited or no network connectivity.
    - **Snowcone**: The smallest device for edge computing and data transfer.
    - **Snowball Edge**: For petabyte-scale data migration and edge computing.
    - **Snowmobile**: For exabyte-scale data transfers. 

---
#### Networking

- **AWS Direct Connect**: A cloud service solution that creates a dedicated, private network connection from your on-premises data center to AWS, offering a consistent, low-latency, and high-bandwidth alternative to the internet.
- **AWS Site-to-Site VPN**: Connects your on-premises network to your AWS Virtual Private Clouds (VPCs) over the public internet, ideal for securely connecting smaller sites.
- **AWS Transit Gateway**: Simplifies your network by acting as a transit hub that connects your VPCs and on-premises networks. 

---

#### Management and monitoring

- **AWS Systems Manager**: Acts as an operational hub for your hybrid environment, allowing you to manage and automate tasks across your EC2 instances and on-premises servers.
- **Amazon CloudWatch**: Provides monitoring for your hybrid environment by collecting metrics and logs from both AWS services and on-premises resources.
- **AWS CloudFormation**: Helps you manage your infrastructure as code, consistently provisioning and updating resources across your hybrid environmen