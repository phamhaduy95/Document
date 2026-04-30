#### Introduction

ML service giúp tìm kiếm và tối ưu hóa các configuration cho AWS resources nhằm tìm ra rightsizing cho resource (instance type cho EC2 hoặc RDS)

Scope của AWS compute Optimizer bao bồm: 

-  EC2 instance~~
- Lambda functions
- Auto Scaling Group
- EBS volume

AWS Compute Optimizer sẽ dùng các metric CPU, memory v IO usage từ CloudWatch thu thập trong 14 ngày gần nhất để tiến hành phân tích và đưa ra cảnh báo.

AWS sẽ phân tính các metric logs thu thập từ AWS CloudWatch như CPU, memory usage, network.
#### Application

Excellent for Rightsizing: 
- instance type phù hợp
- storage type

Ví dụ: Ta dùng `m5.large` instance để run 1 ứng dụng và muốn dùng AWS Compute Optimizer kiểm tra liệu instance có phù hợp với nhu cầu sử dụng. Sau khi phân tính các thông số quan trọng,  AWS compute Optimizer sẽ đưa ra khuyến nghị dùng instance thấp hơn `t3.medium` giúp giảm thiểu chi phí mà vẫn đáp ứng nhu cầu sử dụng.
#### Pricing

Free to use
