#### Introduction

với AWS lambda, user có thể tạo một function thực hiện một tác vụ 
AWS lambda là dịch vụ serverless, AWS tự động cung cấp các compute resource để chạy function. 
AWS lambda sử dụng event driven architect  mỗi một lambda function cần dựa vào 1 event để trigger.

#### Configuration

Các configuration của AWS lambda mà user có thể điều chỉnh bao gồm:

- **Function name**: tên của function, name là unique trong 1 region. Trong trường hợp user đổi tên, AWS sẽ tạo 1 function mới với tên mà user muốn đổi.
- **Runtime**: các runtime execute code trong function. Các runtime được hỗ trợ mặc định bao gồm Node, Python, Java, C#, Go, Rust. Trong trường hợp muốn sử dụng một runtime khác, ngôn ngữ khác, ta hoàn toàn có thể [tạo custom runtime](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-custom.html).
- **Role**: Cung cấp 1 role chứa permission truy cập và sử dụng AWS resource khác cho AWS lambda.
- **Memory**: cung cấp memory trong khoảng 128M đến 10Gig . Lưu ý CPU cung cấp cho function tỉ lệ thuận với số lượng memory được thêm vào. ví dụ tại 1,769 MB, function sẽ có 1 vCPU riêng.
- **Timeout**: thời gian tối đa mà function có thể chạy. Giá trị trong khoảng 1s -> 15phút.
- **VPC**: Nêu VPC mà lambda function muốn được đặt để có thể access vào những private resource như EC2, RDS trong VPC đó.
- **Ephemeral storage:**  kích thước vùng nhớ tạm phân bổ cho từng **execution environment**. User có thể access vùng nhớ này tại **/temp** directory.

#### Quota
Khi dung AWS ta nên lưu ý các giới hạn sau đây:
- Kích thước code không vượt qua 250Mb khi chưa compress và 50Mb khi đã được compress.
- Một lambda function có execution time tối đa là 15 phút. Trong trường hợp function có execution time quá dài ta có thể dùng  AWS Step function break function thành nhiều segment nhỏ hơn.
- Currency limit là 1000 lambda invocation trên 1 account trên 1 region. Tham khảo công thức tính concurrency bên dưới 

#### Scalability

AWS lambda thực hiện scaling theo concurrency tức AWS sẽ invoke nhiều function cùng 1 lúc để đảm bảo đáp ứng tải trọng. 

Ta có thể tính số lượng các concurrent function đang chạy theo công thức sau

**Concurrency** = (**average requests per second**) * (**average request duration in seconds**)

quotas cho currency là 1000 concurrent call cho toàn bộ lambda function trong một region. Trong trường hợp user vượt quá quota

> [!note] Lưu ý
> Function sẽ bị throttled  và trả error message  ` 429 TooManyRequestsException` cho các lần gọi vượt hạn mức.
##### reserved concurrency

Trong trường hợp, có nhiều lambda function chạy đồng thời cùng 1 lúc, các function này sẽ cố tranh giành 1000 concurrency quota. Để đảm bảo 1 function quan trong luôn có concurrency, ta có thể cung cấp **reserved concurrency** cho function đó. 

>[!note] Lưu ý 
>Với function có reserved concurrency, function sẽ bị throttle khi nó vượt hạn mức cho phép trong khoảng reserved concurrency được cung cấp

#### provision concurrency

#### Lambda cold start

Khi lambda function mới được gọi lần đầu tiên, AWS phải cần 1 khoảng gian gọi là init phrase để khởi tạo **execution environment**. Các lần gọi function tiếp theo sẽ tận dụng execution environment đã tạo. Khoảng thời gian tạo **execution environment** này còn có tên gọi khác là cold start.

Để giảm thiểu cold start, ta có thể đặt **provisioned concurrency**
#### Pricing

Every time a Lambda function executes, you are charged based on the RAM/CPU  and processing time the function uses.

- Lambda gives you **1M free requests per month**.
- So, **1M requests → $0** (fits in free tier)
- After free tier: $0.20 per 1M requests.

GB-seconds = (Memory in GB) × (Execution time in seconds) × (Number of requests)
#### Use case

You can use Lambda for:

- **Stream processing**: Process real-time data streams for analytics and monitoring. 
- **Web applications**: Build scalable web apps that automatically adjust to demand.
- **Mobile backends**: Create secure API backends for mobile and web applications.
- **IoT backends**: Handle web, mobile, IoT, and third-party API requests.
- **File processing**: Process files automatically when uploaded to Amazon Simple Storage Service.  
- **Database operations and integration examples**: Respond to database changes and automate data workflows.
- **Scheduled and periodic tasks**: Run automated operations on a regular schedule using EventBridge.