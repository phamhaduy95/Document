
#### Introduction

ECS là giải pháp container orchestration trên môi trường cloud cho phép deploy containerized application trên 1 cluster gồm nhiều EC2 instance và RDS. Các instance trong ECS cluster có thể nằm trên một hay nhiều Availability Zone trong cùng 1 region.

ECS sử dụng cloud-native orchestrator riêng do AWS phát triển có các ưu điểm như dễ sử dụng, tương tích cao với AWS native tuy nhiên lại thiếu tính flexible so với Kubernetes.

ECS được tính hợp sẵn với các AWS service sau:

- **Elastic Load Balancer,  Auto Scaling** - phục vụ scaling và load balancing cho ECS services
- **AWS IAM** - access security
- **AWS CloudWatch, AWS CloudTrail**: monitoring và logging.
- **Elastic File System**: shared volume storage cho cluster.

Lưu ý các EC2 nếu không sử dụng ECS-optimized AMI, ta cần install ECS agent
Always keep 3 tasks running.
#### ECS Task definition

Giống vối manifest file của Kubernetes, ECS dùng task definition để define state cho containerized cluster.
Task definition bao gồm các thành phần chính sau:

- `family` - tên phân biệt các task definition. support đánh version ví dụ `web-app:1`, `web-app:2`
- `container definitions`- define các tham số hoạt động cho từng containerized service ( image, CPU, RAM, port mapping, environment variables, ...)
- `volume`- shared volume của cluster.

Note: Task definition là một immutable file. Muốn thay đổi thông số hoạt động cho EC2, user phải tạo và cung cấp một task definition mới để ECS triển khai sự thay đổi.

```JSON
{
  "family": "web-app:2",
  "requiresCompatibilities": ["FARGATE"],
  "networkMode": "awsvpc",
  "cpu": "256",
  "memory": "512",
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "taskRoleArn": "arn:aws:iam::123456789012:role/ecsTaskRole",
  "containerDefinitions": [
    {
      "name": "web-container",
      "image": "nginx:latest",
      "essential": true,
      "portMappings": [
        { "containerPort": 80, "hostPort": 80, "protocol": "tcp" }
      ],
      "environment": [
        { "name": "ENV", "value": "prod" }
      ],
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/my-web-app",
          "awslogs-region": "us-east-1",
          "awslogs-stream-prefix": "web"
        }
      },
      "healthCheck": {
        "command": ["CMD-SHELL", "curl -f http://localhost/ || exit 1"],
        "interval": 30,
        "timeout": 5,
        "retries": 3
      }
    },
    {
      "name": "sidecar",
      "image": "amazon/aws-for-fluent-bit:latest",
      "essential": true,
      "firelensConfiguration": {
        "type": "fluentbit",
        "options": { "enable-ecs-log-metadata": "true" }
      }
    }
  ],
  "volumes": [
    {
      "name": "efs-volume",
      "efsVolumeConfiguration": {
        "fileSystemId": "fs-12345678",
        "rootDirectory": "/data"
      }
    }
  ],
  "tags": [
    { "key": "Project", "value": "WebApp" }
  ]
}
```

#### ECS Launch Type

Các Launch type supported cho ECS:

1. **AWS Fargate** -AWS Fargate là serverless service tự động quản lý cũng như cung cấp infrastructure cho ECS. Fargate yêu cầu `awsvpc` networking và một lượng vCPU và memory tối thiểu.
2. **EC2 launch type** - chạy ứng dụng trên một EC2 cluster. User có thể điều khiển vị trí đặt của từng container trên nhiều EC2 instances và nhiều AZ khác nhau thông qua **task placement strategy**.
3. **External launch type** - Self-managed ECS on-premises deployments on an organization’s own hardware resources.

#### Auto Scaling

ECS thực hiện scaling theo task hoặc theo số lượng EC2 instance.

#### Load Balancing

You can also optionally run your service behind a load balancer. When you associate a load balancer with an ECS service, ECS dynamically registers running tasks as targets in the load balancer's target groups.

ECS supports **dynamic host port mapping** for EC2 launch types (allowing multiple tasks on the same host with random host ports) and **fixed ports** for Fargate (using `awsvpc` networking mode, where each task gets its own ENI)

Application Load Balancers support path-based routing and priority rules (so that multiple services can use the same listener port on a single Application Load Balancer)

#### Deployment mode

Ta có thể deploy ECS theo 2 cách sau:

| Type                            | Description                                                             | Use Case                              |
| ------------------------------- | ----------------------------------------------------------------------- | ------------------------------------- |
| **Rolling Update**              | Replace old tasks gradually with new ones.                              | Default ECS deployment type.          |
| **Blue/Green (via CodeDeploy)** | Create a new set of tasks behind a new target group and switch traffic. | Safer deployments with zero downtime. |
|                                 |                                                                         |                                       |
|                                 |                                                                         |                                       |
