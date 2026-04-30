#### Introduction

**AWS Transit Gateway** đóng vai trò như một central hub hoặc điểm trung gian của tất cả các kết nối từ multiple VPCs với on-premises networks. 

AWS Transit Gateway giúp tiết kiệm cả chỉ phí, thời gian và công sức do loại bỏ các bước setup service và infra cần thiết để kết nối ra internet hay từ VPC ->VPC, từ region này sang region khác

Các network được kết nối thông qua Transit Gateway cần đảm bảo không bị trùng CIDR block

**AWS Transit Gateway** mang đến nhiều ưu điểm sau đây:

- có thế kết nối hàng ngàn VPCs trong một region với nhau thông qua 1 cổng kết nối trung gian duy nhất (tránh tạo nhiều VPC peering).
- hỗ trợ hybrid connectivity đến on-premise network thông qua VPNs và **AWS Direct Connect** 
- integrates with **AWS Global Networks** for multi-Region and multi-account management
- integrates with **AWS Network Firewall** for inspection

Để sử dụng **AWS Transit Gateway**, ta cần tạo 1 route table có route điều hướng traffic đến AWS Transit Gateway.


