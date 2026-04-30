VPC Peering cho phép ta tạo 1 kết nối dạng peer-to-peer từ 1 VPC này đến 1 VPC khác mà không cần thông qua mạng internet.

Key features:
- **Point-to-Point Connection**: Connects exactly two VPCs (one-to-one).
- **Intra- or Inter-Region**: Supports VPCs in the same Region or different Regions.
- **No Single Point of Failure**: Direct routing, no managed service or device in the middle.

Không hỗ trợ CIDR ranges overlapping giữa các VPCs.

VPC peering không hỗ trợ tính chất bắc cầu vi dụ, cho VPC A->VPC B và VPC B ->VPC C, VPC A chưa peering với VPC C 
#### Pricing

No hourly charges; only data transfer costs (~$0.02/GB intra-Region, both directions).

#### Integration with PrivateLink
Use VPC Peering to connect two VPCs, with one hosting a PrivateLink endpoint for an AWS service or custom application. 
Less scalable than Transit Gateway for multiple VPCs but simpler for small setups.

    
