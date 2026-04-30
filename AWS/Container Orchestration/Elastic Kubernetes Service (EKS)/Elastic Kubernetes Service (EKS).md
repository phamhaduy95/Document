Amazon Elastic Kubernetes Service (EKS) is a fully managed Kubernetes service provided by Amazon Web Services (AWS). EKS automates the provisioning, configuration, and scaling of Kubernetes clusters, handling tasks like API server updates, `etcd` management, and worker node orchestration.

- **Managed Control Plane**: master nodes are managed and controlled by AWS itself, you manage only worker nodes (via EC2 or Fargate).
- **Security**: Integrates with IAM for role-based access, AWS PrivateLink for private clusters, and Amazon EKS Pod Identity for fine-grained permissions.
- **Scalability**: Auto-scaling for nodes and pods; supports up to thousands of nodes per cluster.
- **Observability**: Built-in integration with Amazon CloudWatch for logs/metrics, AWS X-Ray for tracing, and Prometheus for monitoring.
- **Networking**: Uses Amazon VPC for pod networking, with options for AWS Load Balancer Controller for ingress/egress.
- **Storage**: Supports Amazon EBS, EFS, and `FSx` for persistent volumes.
#### Integration
Amazon EKS is integrated with AWS services such as Amazon CloudWatch, EC2 Auto Scaling groups, IAM, and ELB Application Load Balancers:  
- **Amazon CloudWatch** logs are directly updated from the EKS control plane audit and diagnostic logs.  
- **EC2 Auto Scaling** via **Kubernetes Cluster Autoscaler** with Auto Scaling groups and Launch templates.  
- The AWS **Load Balancer Controller** manages AWS ELB Load Balancers for each Kubernetes cluster.  
- AWS IAM security creates IAM roles for role-based access control (RBAC). Access to an EKS cluster using IAM entities is enabled by the AWS Authenticator for Kubernetes, which allows authentication to a Kubernetes cluster.

The EKS control plane has a minimum of two highly available API server instances.  
There are also three `etcd` instances hosted across three availability zones within each AWS region.
EKS clusters.
Pods can be deployed as self-managed nodes, EKS-managed node groups, or using AWS  
Fargate
### Pricing

EKS follows a pay-as-you-go model with no upfront fees.
- **Control Plane**: $0.10 per hour per cluster (~$73/month for always-on).
- **Worker Nodes**: Charged based on underlying EC2 instances (e.g., t3.medium at ~$0.0416/hour) or Fargate vCPU/memory usage ($0.04048/vCPU-hour + $0.004445/GB-hour).
- **Data Transfer**: Standard AWS rates (e.g., $0.09/GB out to internet).
- **Add-ons**: Free for core features; extras like EBS volumes add costs.
