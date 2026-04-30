#### Introduction

**AWS Elastic Load Balancer** (ELB) là dịch vụ cân bằng tải, giúp phân phối lưu lượng truy cập mạng đến nhiều máy chủ hoặc tài nguyên để đảm bảo hiệu suất, độ tin cậy và khả năng mở rộng cho các ứng dụng. 

Các điểm mạnh của ELB:
- **Automatic Scaling**:  thay đổi capacity theo traffic demand.
- **High Availability**: điều hướng traffic đến nhiều Availability Zones.

Ta thường kết hợp **ELB** với **EC2 Auto Scaling** nhằm tối ưu cho cả hai yếu tố *availability* và *scalability*. Trước khi các instance bị terminate, **ELB** sẽ điều hướng tải ra khỏi các instance này đến các instance còn hoạt động khác.

**Elastic Load Balancing** cung cấp 3 loại balancer chính: 
 
 - **Application Load Balancer** phục vụ HTTP và HTTPS (layer 7)
 - **Network Load Balancer** cho TCP traffic (layer 3 và 4)
 - **Gateway Load Balancer** phục vụ các third-party load balancers hỗ trợ GENEVE protocol như Nginx, Cisco. 

ELB supports IPv4 and IPv6 for Internet-facing load balancers; for internal load balancers, it supports only IPv4.
#### Application Load balancer

The application load balancer operates at the Application layer (Layer 7 OIS model).
Layer 7 protocols permit it manage and route incoming public traffic from the Internet.

- **Dynamic Traffic Routing** : Routing can be based on host name, path, query string parameter, HTTP headers, source IP address, or port number
- **SSL/TLS traffic decryption**
- **Server Name Indication (SNI)**: Application load balancers support hosting multiple certificates per ALB, enabling multiple websites with separate domains to be hosted by a single ALB
- **Dynamic port mapping** : Application load balancers support load-balancing containers running the same service on the same EC2 instance where the containers are hosted.
- **Connection draining**: close connections for unhealthy and deregistered instances and keeps remaining open for using.  
- **Cross-zone load balancing**: distribute incoming traffic requests evenly across the registered targets in the multiple availability zones (min is 2 AZs) 
- **User authentication:** ALB integrates with AWS Cognito which allows both web-based and enterprise identity providers to authenticate through the ALB.

Note: The Classic Load Balancer does not support **Dynamic port mapping**. You cannot run multiple copies of a task on the same instance, because the ports would conflict
##### Network Load Balancer
Similar to **Application Load Balancer**, **Network Load Balancer** (NLB) is also a load balancing service that distributes incoming traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses, in one or more AZ. However, **Network Load Balancer** provides TCP and UDP load balancing at Layer 4 of the OSI stack.

Some important features of NLB:

- **TLS offloading**: Client TLS session termination is supported, allowing TLS termination tasks to be carried out by the load balancer.  
- **Server Name Indication (SNI**): Serves multiple websites using a single TLS listener.  
- **AWS Certificate Manage**r: Manages server certificates.  
- **Sticky sessions:** Can be defined per target session.  
- **Static IP address**: A static IP address is provided per AZ.  
- **EIP support**: An Elastic IP address can be assigned for each AZ.  
- **DNS fail-over**: If there are no healthy targets available, Route 53 directs traffic to load balancer nodes in other AZs.  
- **Route 53 integration**: Route 53 can route traffic to an alternate NLB in another AWS region
- **extremely high throughput**: an NLB can scale and handle millions of requests per second
- **long-running connections**: ideal for WebSocket applications

##### Gateway Load Balancer
load balancer that sits at Layer 3 of the OSI model
Use GENEVE protocol to exchange traffic 

#### ELB target group
ELB target group là cluster gồm nhiều compute instance sẽ được Elastic Load Balancing cân bằng tải và điều hướng traffic đến. ELB hỗ trợ các target group sau:
- **instance**: route traffic đến IP của các EC2 instance có sẵn trong AWS account.
- **IP address**:  Routes traffic đến một nhóm IP addresses từ EKS containers, on-premises servers, hoặc non-EC2 resources trên cùng 1 VPC.
- **lambda function**: Routes HTTP/HTTPS requests đến AWS Lambda functions
- **Application load Balancer**: route traffic từ 1 Gateway Load Balancer
####  Listener 

In AWS, Elastic Load Balancers (ELBs) use listeners to manage incoming traffic by defining how the load balancer receives and **routes client requests** to backend targets. 

A rule consists of a set of conditions and an action. When the ALB receives a request, it evaluates the conditions in the rule to determine whether the action should be taken.

Multiple rules can be created for an ALB, and each rule can have multiple conditions. The order of the rules is important because the ALB evaluates the rules in the order in which they are specified.
One listener can have multiple rules.

**Rules**:
- **Default Action**: Specify what happens if no rules match (e.g., forward to a target group, return a 404 response).
- **Add Rules**: Define conditions for routing:
	- **path-pattern**: This routing option is based on the path pattern of the URL
	- **host-header**:  This routing option based  on the domain name contained in the host header (SNI).
	- **source-IP**: This routing option is based on the source IP address for each request.
	- **http-header**: This routing option uses the HTTP headers
	- **query-string**: This routing option is based on key/value pairs or values in the query name configuration
- **Actions**:
	- **Forward**: Send traffic to a target group. support traffic distribution (% base) for canary deployment,
	- **Redirect**: Redirect HTTP to HTTPS or to another URL.
	- **Fixed Response**: Return a custom HTTP response (e.g., 503 for maintenance).
	- **Authenticate**: Integrate with AWS Cognito or OIDC for user authentication.
	- **Priority**: Rules are evaluated in order (1 to highest); lower numbers take precedence.
- **Advanced Settings**:
	- **TLS Settings**: For HTTPS, configure cipher suites and TLS versions (e.g., TLS 1.2 or higher).
	- **Client IP Preservation**: Enable to pass client IPs via X-Forwarded-For headers.
	- **Timeout Settings**: Set connection idle timeout (1–4000 seconds).

**Use Cases**:
- Redirect HTTP to HTTPS for secure browsing.
- Canary deployment and A/B testing.
- Serve multiple websites using a single TLS listener by leveraging Server Name Indication (SNI) and advanced routing rules
#### Sticky sessions
scope: apply for entire target group:

An ELB supports **sticky sessions**, which allow the load balancer to bind the user’s active session to a specific EC2 instance. With sticky sessions enabled on a load balancer, after a request is routed to a target, a cookie is generated by the load balancer or application and returned to the client, ensuring that requests are sent to the EC2 instance where the user session is located
#### Pricing

The pricing for AWS Application Load Balancer (ALB) is based on a combination of hourly usage and Load Balancer Capacity Units (LCUs)

A fixed rate for each hour or partial hour that the ALB is running.

**LCU Charge**: A variable charge based on the number of LCUs consumed, calculated as the maximum of four dimensions:
- **New Connections**: Number of newly established connections per second.
- **Active Connections**: Number of concurrent active connections.
- **Processed Bytes**: Amount of data processed by the ALB.
- **Rule Evaluations**: Number of rules processed per request (based on routing rules configured).
#### Security

- **Supports AWS WAF** for web application firewall rules.
- **X-Forwarded-For headers** for client IP visibility.
- Assign **security groups** to control inbound/outbound traffic 
- Integrates with OIDC, SAML, LDAP, Amazon Cognito, or social providers (e.g., Google, Facebook) for user authentication.
#### Configuration
**Scheme**: Choose Internet-facing (public) or Internal (private)
**Availability Zones**: Choose at least two subnets in different Availability Zones for high availability.
**Listeners**: Add listeners for protocols/ports (e.g., HTTP:80, HTTPS:443).
- **Rules**: Define routing rules
- **Default Action**: Forward to a target group, redirect, or return a fixed response.
**Security Settings**:
- **Security Groups**: Assign security groups to control inbound/outbound traffic
- **SSL/TLS Certificates**: For HTTPS, select an ACM (AWS Certificate Manager) certificate or upload your own.
- **TLS Settings**: Choose cipher suites and TLS versions (e.g., TLS 1.2 or higher).
**Target Groups**:
- **Target Type**: Choose **Instance** (EC2), **IP** (for containers or on-premises), **Lambda**, or **ALB** (chaining).
- **Protocol/Port**: Set the protocol (HTTP/HTTPS) and port for targets.
- **Health Checks**:
    - Path (e.g., /health), protocol, and thresholds 
    - Healthy/unhealthy thresholds, interval, and timeout.
- **Attributes**:
    - **Deregistration Delay**: Time to wait before deregistering a target (0–3600 seconds).
    - **Stickiness**: Enable session stickiness (cookie-based) with a duration (1 second–7 days).
    - **Slow Start**: Gradually ramp up traffic to new targets (30–900 seconds).

**Advance setting:**
- Connection Idle Timeout: Time before closing idle connections (1–4000 seconds).
- AF Integration: Associate with AWS Web Application Firewall for security.
- Cross-Zone Load Balancing: Distribute traffic evenly across all AZs (enabled by default).

#### TSL termination at ELB
1. The **client** sends an encrypted HTTPS request to the load balancer (e.g., an AWS Application Load Balancer).
2. The **load balancer** receives the request and, using the TLS certificate you installed on it, **terminates the TLS session** by decrypting the request.
3. The load balancer forwards the (now unencrypted) request to one of the backend servers.
4. The backend server processes the request and returns an unencrypted response to the load balancer.
5. The load balancer re-encrypts the response and sends it back to the client

Note:
Proxy protocol for TCP/SSL carries the source (client) IP/port information. The Proxy Protocol header helps you identify the IP address of a client when you have a load balancer that uses TCP for back-end connections. You need to ensure the client doesn’t go through a proxy or there will be multiple proxy headers. You also need to ensure the EC2 instance’s TCP stack can process the extra information

ELB nodes have public IPs and route traffic to the private IP addresses of the EC2 instances. You need one public subnet in each AZ where the ELB is defined and the private subnets are located

