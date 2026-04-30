#### Introduction

**Amazon SNS** use **Fan-Out Architecture**, where a single message published to a ***topic*** is delivered to **multiple subscriber**s (e.g., multiple SQS queues or Lambda functions).

Each SNS topic can have a choice of subscribers such as

- **AWS Lambda** - An SNS notification can be linked to a function to carry out one or more tasks at AWS  
- **Amazon SQS** - Queues can notify SNS topics that messages have been delivered  
- **HTTP/S endpoints** - Deliver SNS notifications to a specific URL  
- **Email** - Email subscribers using push notifications
- SMS

SNS supports the `PublishBatch` API, allowing up to 10 messages to be published in a single request, reducing API calls and improving throughput.

![[Pasted image 20251009232829.png]]

#### Security

- **IAM Policies**: Control access to SNS topics and actions (e.g., Publish, Subscribe).
- **Topic Policies**: Define which AWS accounts or users can publish or subscribe to a topic.
- **Encryption**: SNS supports server-side encryption using AWS KMS (Key Management Service) for topic messages.

#### Scalability and Availability

SNS is deployed across multiple Availability Zones (AZs) within an AWS region, ensuring redundancy and fault tolerance.

#### Integration with CloudWatch

Amazon SNS is integrated with Amazon CloudWatch. Utilizing SNS notifications as part of your workload design lets you decouple your application communications and react when changes occur to workloads or associated AWS services; for example, changes in a DynamoDB table being monitored by CloudWatch can trigger an alarm when data values increase or decrease.

#### Application

- **Real-Time User Notifications**: SNS is used to send real-time notifications to users via email, SMS, or mobile push notifications for events like account updates, alerts, or confirmation
- **Event-Driven Architectures:** SNS integrates with other AWS services to trigger actions or workflows in response to events, enabling event-driven architectures
- **Alerting and Monitoring**: SNS is used to send alerts or notifications based on system health, performance metrics, or operational events, often integrated with Amazon CloudWatch
- **Microservices Communication**: SNS facilitates communication between microservices by broadcasting events to multiple services, enabling decoupled and scalable architectures.




Key Features  
• Topic-Based Pub/Sub Model: Publishers send messages to a topic, and  
multiple subscribers receive the messages simultaneously.  
• Multiple Protocols: Supports a variety of endpoints including  
HTTP/HTTPS, Lambda functions, email, SMS, and SQS.  
• Fan-Out Pattern: Allows the same message to be sent to multiple  
destinations, such as delivering a notification to a mobile app and  
triggering a Lambda function simultaneously.  
• Filtering and Message Attributes: Allows subscribers to filter  
messages they receive based on specific attributes, reducing  
unnecessary processing




When using **Amazon SNS Mobile Push**, each mobile device (with its unique registration token from APNs, FCM, ADM, etc.) must be **registered as an SNS platform endpoint** before you can send it push notifications.

You do this by calling the **`CreatePlatformEndpoint`** API for each device.