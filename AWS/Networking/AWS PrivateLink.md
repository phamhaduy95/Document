#### Introduction 
Cho phép ta tạo 1 kết nối bảo mật từ 1 VPC đến AWS service, third-party services, hoặc 1 VPC endpoint service ở một VPC, một region hay một account khác mà không cần thông qua internet.


Use Cases bao gồm:
- Access AWS services (e.g., SNS, SQS, API Gateway) privately from a VPC.
- Connect to a custom application (e.g., EC2 instances) in another VPC or AWS account.
- Share services with other accounts or VPCs securely.
- Integrate with Transit Gateway for multi-VPC or hybrid connectivity.
- 
#### VPC Endpoint
**VPC Endpoint** là thành phần của **AWS PrivateLink** đóng vai trò như 1 điểm kết nối cho **AWS Private Link.**

Với các EC2 instance đã có ENI gắn 1 private ID riêng. 
Muốn kết nối với các service ta phải tạo 1 VPC endpoint và dùng private link 

**VPC Endpoint** có các loại chính sau:
1. **Gateway endpoint**:
	- **Target** - chỉ áp dụng cho S3 và DynamoDB. 
	- **Cost**  - Miễn phí khởi tạo chỉ tính phí cho data ingress.
	- **Implement** - tạo 1 route table trỏ đến địa chỉ của S3 bucket hoặc dynamoDB
2. **Interface Endpoint:**
	- **Target**: áp dụng cho các service khác, VPC hoặc EC2 instance;
	- **Cost**: hourly charges và data processing fees
	- hỗ trợ private DNS names
#### Best Practices

- Use Gateway Endpoints for S3/DynamoDB to save costs.
- Place Interface Endpoints in multiple subnets for high availability.
- Hybrid Scenarios: Combine with AWS Transit Gateway to route on-premises traffic to AWS services via PrivateLink.

#### Integration with Transit Gateway

Deploy one Interface Endpoint or Gateway Endpoint in a hub VPC and use Transit Gateway to route traffic from other VPCs, reducing endpoint costs.
Example: 5 VPCs access an S3 Gateway Endpoint in one VPC via Transit Gateway
