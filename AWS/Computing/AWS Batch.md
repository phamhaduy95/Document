**AWS Batch** helps you to run batch computing workloads on the AWS Cloud at any scale without manual management for infra. 
AWS Batch leverage container-base service ECS  for computing. You can choose between Fargate or EC2 instances 

Advantages of using AWS:
- **Scale compute resources automatically**
- **Optimize computing cost**: Reduce costs by optimizing computing job distribution based on volume and resource requirements

 **AWS Batch key components:**
 
- **Jobs**: Jobs are the unit of work that's started by AWS Batch. Jobs can be invoked as containerized applications that run on Amazon ECS container instances in an ECS cluster.
- **Job Definitions**: A blueprint for your jobs. It specifies the **container image**, CPU and memory requirements, environment variables, and any other settings for how the job should run.
- **Job Queues**: Where you submit your jobs. The jobs reside here until the AWS Batch scheduler assigns them to a compute environment. You can use queues to prioritize jobs or separate them by function.
- **Compute Environments**: The pool of compute resources where jobs are executed. They can be configured to use Amazon EC2 instances, AWS Fargate, or Amazon EKS. it can be managed environment  unmanaged environment.
#### Application

**AWS Batch** is excellent for _batch workloads_ such as nightly processing jobs, analytics crunching, bulk file conversions, or any kind of **large-scale parallel computation** such as:

- **High-performance computing**: Running large-scale scientific or engineering simulations for fields like weather forecasting, financial risk modeling, and drug screening.
- **Machine learning**: AWS Batch is used to train machine learning models or run batch inference on large datasets, especially when real-time processing is not required.
- **Scientific Computing and Simulation**s: AWS Batch is used for compute-intensive simulations or calculations in fields like bioinformatics, physics, or financial modeling.
- **Media processing**: AWS Batch processes large media files (e.g., videos, images, audio) for tasks like encoding, transcoding, or rendering
- **Big data processing**: Executing ETL (Extract, Transform, Load) jobs and running large-scale data analytics task

#### Pricing
There is no additional charge for AWS Batch. You pay for AWS resources you create to store and run your application. You can use your Reserved Instances, Savings Plan, EC2 Spot Instances, and Fargate with AWS Batch by specifying your compute-type requirements when setting up your AWS Batch compute environments