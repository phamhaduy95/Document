#### Introduction

CloudWatch là dịch vụ giúp thu thập và lưu trữ  các thông số hoạt động (metric) của các service và resource thuộc hay không thuộc AWS. Ngoài ra CloudWatch còn có tính năng hiện thị trực quan (graph view) cho.

Một số service mặc định tự động enable CloudWatch để có thể hoạt động được như Auto Scaling hoặc ECS cần có metric về EC2 performance để có chiến thuật scaling phù hợp.

Ngoài các metric sẵn có, AWS CloudWatch còn cho phép hỗ trợ tạo custom metric cho cả các resource từ cloud provider khác và on-premise.

> [!note]
> Lưu ý: CloudWatch là `regional service`. Chính vì thế, ta cần tạo và config CloudWatch riêng  mỗi một region.
>
#### CloudWatch metrics

Các metric phân biệt với nhau thông qua các giá trị sau `namespace`, `name` và `dimension`.
ta có ví dụ về metric đo CPUU cho EC2:

- **namespace**: `AWS/EC2` (phân biệt giữa application, AWS service hay custom metric)
- **name**: `CPUUtilization` (tên thông số được đo đạc)
- **dimension**:  `InstanceId` and `InstanceType`. (dùng phân biệt giữa các instance với nhau)

AWS sẽ đo đạc và thu thấp dữ liệu cho các metric dưới dạng time series bao gồm nhiều `data point`. Mỗi point sẽ có `timestamp`, `value` và `unit of measure`.

#### Basic monitoring and detailed monitoring in CloudWatch

AWS CloudWatch hỗ trợ 2 loại monitoring chính:
basic monitoring:

- chỉ dùng cho các metric hỗ trợ mặc định.
- thu thấp dữ liệu mỗi 5 phút
detailed monitoring:
- thu thập dữ liệu mỗi 1 phút
- tuy vào service khác nhau, detail monitoring cho phép hiển thị nhiều thông tin hơn.

#### custom metric

với custom metric, user có thể:

- chọn resolution giữa standard (1 phút) và high (1s)
- tạo custom `namespace`, `name` và `dimension`.

custom metric pricing dựa vào lượng data ingest vào AWS CloudWatch và các API integration với CloudWatch vi dụ `PutMetricData`

ví dụ về publish custom metric qua CLI

``` bash
aws cloudwatch put-metric-data --metric-name PageViewCount --namespace MyService --value 2 --timestamp 2016-10-20T12:00:00.000Z
aws cloudwatch put-metric-data --metric-name PageViewCount --namespace MyService --value 4 --timestamp 2016-10-20T12:00:01.000Z
aws cloudwatch put-metric-data --metric-name PageViewCount --namespace MyService --value 5 --timestamp 2016-10-20T12:00:02.000Z
```


