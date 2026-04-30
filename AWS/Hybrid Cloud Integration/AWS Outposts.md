Bring native AWS services and infrastructure to your on-premises environments

AWS sẽ install các physical hardware tương thích với AWS system về on-premise cho phép user sử dụng các dịch vụ cơ bản (compute, network, extend VPC, management tools).
Phù hợp với các nhu cầu
latency thấp
data residency: sensitive data cần để 1 chỗ


AWS Outposts is ==a family of fully managed solutions that extend AWS infrastructure, services, APIs, and tools to customer premises==. It provides a consistent hybrid cloud experience, allowing you to build and run applications on your own on-premises equipment using the same cloud technologies and management tools you use in an AWS Region. This is ideal for workloads that need to be run locally due to low-latency requirements, local data processing, or data residency needs. 

Key features

- **Fully managed infrastructure**: AWS owns, operates, monitors, and manages the Outposts hardware for you. This includes delivery, installation, monitoring, maintenance, and software updates, significantly reducing your operational overhead.
- **Consistent hybrid experience**: You use the same APIs, management console, tools, and services on-premises as you do in the cloud, standardizing your development and operations workflows.
- **Low latency and local data processing**: Applications can use local compute and storage resources to meet low-latency needs for use cases like factory automation, medical imaging, or real-time analytics.
- **Data residency**: Outposts allows you to control where your data resides, helping you meet legal, contractual, and regulatory requirements.
- **Seamless VPC extension**: You can extend your Amazon Virtual Private Cloud (VPC) to your on-premises environment. Resources on the Outpost communicate with other resources in the AWS Region using private IP addresses within the same VPC.
- **High availability**: Outposts racks are designed with built-in redundancies, including power supplies and network switches, to support high-availability deployments

#### Services commonly supported on Outposts

- **Compute**: Amazon EC2 instances, Amazon Elastic Container Service (ECS), and Amazon Elastic Kubernetes Service (EKS).
- **Storage**: Amazon EBS volumes, Amazon S3 on Outposts, and Amazon EBS Snapshots.
- **Databases**: Amazon RDS on Outposts (for SQL Server, MySQL, and PostgreSQL).
- **Networking**: Amazon VPC subnets, Application Load Balancers, Route 53 Resolver, and the ability to extend your VPC on-premises.
- **Management and other services**: Access to regional services like AWS CloudFormation, Amazon CloudWatch, and AWS CloudTrail


AWS Outposts rack

- A **standard 42U rack** that is fully assembled by AWS and installed in your data center.
- It comes with a mix of compute and storage capacity, including Amazon EC2 and Amazon EBS, and can be configured with S3 on Outposts.
- Designed for organizations that need to run a broad range of AWS services locally at scale.
- Scaling can involve single racks or multiple rack deployments to create large pools of on-premises capacity. 

AWS Outposts servers

- A smaller **1U or 2U server** that can be installed in a standard 19-inch rack.
- It provides local compute and networking services for edge locations or sites with limited space or smaller capacity needs, such as branch offices or retail stores.
- While smaller, it still runs AWS-managed infrastructure on the same Nitro System as the cloud.
- Uses a Local Network Interface (LNI) to communicate with the on-premises network.