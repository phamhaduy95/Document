
S  
The Fan-Out pattern is a design pattern commonly used in distributed  
systems to split a task into multiple parallel sub-tasks, distribute them  
across multiple services or components, and then aggregate the results
This  
pattern helps improve the scalability, efficiency, and performance of  
applications by enabling them to handle large workloads concurrently.


Use Cases of the Fan-Out Pattern  
Batch Processing: Used for processing large datasets by dividing the data  
into smaller chunks and distributing them across multiple processing nodes.  
For example, processing large log files or performing bulk image  
processing.  
Microservices Communication: In microservices architecture, one service  
can fan out a request to multiple downstream services to retrieve or process  
different pieces of data. For example, an e-commerce application might fan  
out a product details request to inventory, pricing, and review services.


Example of Fan-Out Pattern  
Suppose an e-commerce application needs to process multiple customer  
orders simultaneously. Using the Fan-Out pattern, the application can break  
down the orders into individual sub-tasks, such as processing payments,  
updating inventory, and sending confirmation emails. Each sub-task is  
assigned to different worker services, and these services process the subtasks concurrently. Once all sub-tasks are complete, the results are  
aggregated to confirm that all orders have been processed successfully.  
Fan-Out Phase: A single service receives multiple customer orders. It  
breaks these orders into sub-tasks (e.g., process payment, update inventory).  
Each sub-task is sent to different services or queues for parallel processing.  
Fan-In Phase: The system waits for all sub-tasks to complete. The results  
from each sub-task are aggregated to produce the final outcome




You can use SNS and SQS in combination to Fan Out and make system  
highly scalable. Once you push an event to an SNS topic, then SQS that are  
subscribers to the topic receives the event. From there each SQS queue,  
consumer can consume their events.