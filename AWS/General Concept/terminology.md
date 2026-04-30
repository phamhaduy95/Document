retention
ingestion
higher fidelity resolution of metrics

Stateless 

■ Scalability: Because stateless web and application servers do not maintain user,  
session, or application data state, they can be easily scaled horizontally by simply adding more instances to handle additional traffic.  
■ Fault tolerance: Stateless workloads can continue to operate even if an individual instance fails. This can help improve the overall reliability and availability of the application.  
■ Flexibility: Stateless applications that don't maintain user and application state  
information can be deployed on any number of servers


The application state (orders and requests, for example) is stored in a messaging queue, such as Amazon Simple Queue Service (SQS), discussed later in this  
chapter.

User session information can be stored redundantly and durably in an Amazon  
ElastiCache for Memcached or Amazon ElastiCache for Redis in-memory  
key-value store

High availability refers to hosted workloads being always available, regardless of the  
situation or circumstances that happen from time to time when running workloads  
in the cloud.
fault tolerance. Fault-tolerant cloud services and workloads are designed to continue operating when problems occur, without failing or  
experiencing a loss of data

Resiliency is the capability of a system or component to withstand external stresses  such as sudden increases in load, attacks from malicious actors, or environmental  factors such as extreme temperatures or natural disasters. A resilient system has been designed for high availability operation across multiple availability zones and can recover quickly from such stresses and continue to function without significant disruption. Resiliency is present because of high availability


A managed service is an AWS service that is built, maintained, and patched by AWS

Stateful data: Data includes user account information, purchases, history, refunds, games played, high scores, music listened to, or music downloaded.  
Stateless data: Data includes user session information, such as information on browsing for products, browsing for games, reviewing account information, or searching for music

There are two common ways to manage AWS user sessions:  
■ stickiness can be selected, generating a load balancer session cookie.  
■ Distributed session management: Another way to address shared data  
storage for user sessions is to use an in-memory key-value store hosted by  
Amazon ElastiCache deploying either ElastiCache for Redis or ElastiCache  
for Memcached caching the user session state. For a simple deployment with  
no redundancy, you could choose to employ ElastiCache for Memcached,  
but this solution provides no replication of the in-memory nodes. Figure 6-4  
shows the operation of a distributed cache with no built-in replication of the  
user cache. When an end user communicates with an application, the user session information is stored in an Amazon ElastiCache for Memcached cache.  
When Server A fails, the user session is continued on Server B because the  
user session information is stored in ElastiCache for Memcached instead of on  
an application server. For a redundant distributed session solution, you could  
deploy ElastiCache for Redis, which supports replicating user session information between multiple nodes across multiple availability zones (AZs), adding  
redundancy and durability to the cached user session information.