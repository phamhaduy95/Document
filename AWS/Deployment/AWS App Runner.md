AWS App Runner là giải pháp deploy container application một cách nhanh chóng và tiện lợi mà không cần quan tâm đến infrastructure.  Ta chỉ cần cung cấp image hoặc địa chỉ GitHub repository có DockerFile.

AWS App Runner được xây dựng trên AWS Fargate một serverless service cung cấp và scale infra tự động cho các ứng dụng container.  

App Runner: Use for simple, stateless web apps/APIs needing zero infrastructure management. Example question: “Deploy a web app from GitHub with auto-scaling and minimal setup.” (Answer: App Runner.)

Security
App Runner: Fully managed IAM roles, KMS encryption, WAF integration, and Secrets Manager for credentials. VPC connectors for private access.

Availability: available in multiple AWS Regions with high availability across multiple Availability Zones (AZs).

