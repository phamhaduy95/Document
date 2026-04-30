
#### Health Checks & Operations

- **How do health checks work in an ELB, and what parameters can be configured?**
    - The load balancer periodically sends requests to targets to monitor their status. If a target fails the health checks, the load balancer stops routing traffic to it.
    - Configurable parameters include:
        - **Protocol** and **Port** for the health check (can be different from the main traffic port).
        - **Health check path** (for ALBs).
        - **Timeout** (how long to wait for a response).
        - **Interval** (how often to perform checks).
        - **Healthy/Unhealthy threshold counts** (number of consecutive successful/failed checks required to change status).
        - **Success codes** (e.g., HTTP 200 by default for ALBs, but can be customized).
- **What is the meaning of a 504 error from an ELB?**
    - A 504 error (Gateway Timeout) from an ELB indicates that the load balancer sent a request to a registered target but the target did not respond within the configured idle timeout period. This usually points to an issue with the application running on the backend instance, such as it being overloaded or stuck.
- **What is the default idle timeout for an ALB and CLB?**
    - The default idle timeout value for both Application and Classic Load Balancers is 60 seconds. 

#### Integration & Advanced Concepts

- **How does ELB integrate with Auto Scaling Groups (ASG)?**
    - The ELB is configured as the front-end for the ASG. The ASG automatically registers new instances with the load balancer as they are launched during a scale-out event and deregisters them during scale-in or termination.
    - Crucially, you can configure the ASG to use the ELB's health checks (in addition to EC2 health checks). If the ELB marks an instance as unhealthy, the ASG will terminate and replace it, ensuring only healthy instances receive traffic.
- **Explain "sticky sessions" (session affinity) and when you might use them.**
    - Sticky sessions allow the load balancer to bind a user's session to a specific target instance. All requests from that user during the session are sent to the same instance. This is necessary for applications that store session state locally on the server (though a better architectural practice for scalability is to store session state externally in services like ElastiCache or DynamoDB).
- **What is "Cross-Zone Load Balancing" and why is it important?**
    - Cross-zone load balancing distributes incoming traffic evenly across all registered targets in all enabled Availability Zones, regardless of which zone the target or client is in. This helps ensure even traffic distribution and improves application fault tolerance and availability. Without it, one AZ might become overloaded while others are underutilized.