Ta chỉ có thể assign 1 task role và task execution role cho 1 task definition

| Role                    | Purpose                                                               | Example                                     |
| ----------------------- | --------------------------------------------------------------------- | ------------------------------------------- |
| **Task Role**           | Allows the _application inside the container_ to access AWS services. | Example: read from DynamoDB.                |
| **Task Execution Role** | Allows ECS Agent to pull images from ECR and push logs to CloudWatch. | Example: grant `ecr:GetAuthorizationToken`. |

If your ECS task fails to pull an image or push logs, the missing role is **task execution role**, not task role.