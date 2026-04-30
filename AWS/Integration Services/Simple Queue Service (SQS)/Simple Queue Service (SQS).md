#### Introduction

**Communication between services** luôn là một vấn đề tối quan trọng trong kiến trúc microservices. AWS cung cấp giải pháp Simple Queue Service (SQS) cho phép user tạo message channel giúp quản lý, lưu trữ và điều phối message từ service này đến service khác.

Ta có thể hiểu các thức hoạt động của SQS như sau: Simple Queue Message đóng vai trò tiếp nhận và lưu trữ tạm thời toàn bộ message. Các consumer service sẽ thực hiện quá trình polling tìm các message liên quan trong SQS và xử lý. Khi một consumer đang xử lý một message, message đó sẽ được ẩn đi  để trách trường hợp nhiều consumer cùng xử lý một message (prevent duplication).
#### Types of Simple Queue

##### Standard Queue

- **Delivery Guarantee**: At-least-once delivery (a message may be delivered more than once due to distributed nature).
- **Ordering**: Không quan trọng thứ tự đến của message.
- **Throughput**: Nearly unlimited throughput, highly scalable.
- **Use Case**: Applications where high throughput is critical, and occasional duplicates or out-of-order messages are acceptable (e.g., logging, event processing).

##### FIFO Queue

- **Delivery Guarantee**: Exactly-once delivery (duplicates are eliminated using deduplication IDs).
- **Ordering**: Strict ordering within a message group (messages with the same `MessageGroupId` are processed in order).
- **Throughput**: Limited to 3,000 messages per second with batching (300 without batching).
- **Use Case**: Applications requiring strict ordering and no duplicates, such as financial transactions or order processing.

#### SQS message

SQS message là 1 đơn vị  của SQS có nhiệm vụ mang các thông tin cần truyền từ service này đến service khác giúp các service giao tiếp bất đồng bộ với nhau.
Kích thức tối đa của một SQS message là 256KB
Các message có thời hạn tồn tại nhất định (mặc định là bốn ngày, tối đa  là 14 ngày)

#### Visible Timeout

Khi một message được retrieved bởi một consumer, message đó sẽ được tạm ẩn trong SQS để tránh trường hợp nhiều consumer xử lý một message cùng một lúc.  Nếu message chưa được xử lý xong trong thời gian timeout, message sẽ hiện diện lại trong SQS.

Sau khi message được xử lý xong, consumer nên gọi API `DeleteMessage` để loại message đó ra khỏi SQS storage. Với Lambda function, SQS sẽ tự động xóa message trong message store.

Invisible timeout có giá trị mặc định là 30s max là 12 giờ.

Ngoài visible timeout, ta còn có thể đặt **Delivery delay** để ẩn message khi message đó vừa được đưa và SQS queue.

#### Security

Authorization with IAM policy or Queue policy
Queues can be encrypted with server-side encryption (SSE) using keys managed by AWS Key Management Service (KMS)

Encryption in Transit

#### Scalability

Handles unlimited queues and throughput, scaling elastically based on demand—no capacity planning needed.

#### High Resiliency and Durability

Messages are stored redundantly across multiple Availability Zones (AZs) to ensure high availability (99.9%+) and no data loss
All SQS message queues and messages are stored in a single AWS region across multiple AZs providing redundancy and failover.

#### Pricing

SQS is pay-as-you-go: $0.40 per million requests after the first million free per month (Standard queues); FIFO adds $0.50 per million requests. No upfront costs or minimums—billed only for API calls (send, receive, delete).

#### Application

- Offloading intensive tasks for asynchronous processing.
- Processing batch jobs and managing workloads.
- Distribute workloads across auto-scaling workers, like image resizing or data processing.
- Buffer events for microservices, ensuring resilience during spikes.

#### Basic Configuration

- **Visibility timeout** – The length of time that a message received from a queue (by one consumer) won't be visible to the other message consumers. Using the console to configure the visibility timeout configures the timeout value for all of the messages in the queue. To configure the timeout for single or multiple messages, you must use one of the AWS SDKs.
- **Message retention period** – The amount of time that Amazon SQS retains messages that remain in the queue. By default, the queue retains messages for four days. You can configure a queue to retain messages for up to 14 days.
- **Delivery delay** – The amount of time that Amazon SQS will delay before delivering a message that is added to the queue. Delay queues are similar to [visibility timeouts](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html) because both features make messages unavailable to consumers for a specific period of time.
- **Maximum message size** – The maximum message size for this queue.
- **Receive message wait time** – The maximum amount of time that Amazon SQS waits for messages to become available after the queue gets a receive request.
- **Enable content-based deduplication** – Amazon SQS can automatically create deduplication IDs based on the body of the message.
- **Enable high throughput FIFO** – Use to enable high throughput for messages in the queue.
- **Redrive allow policy**: defines which source queues can use this queue as the dead-letter queue.
