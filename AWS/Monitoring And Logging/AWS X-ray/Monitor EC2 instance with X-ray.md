
To enable **AWS X-Ray** tracing for an application running on **Amazon EC2**, two components are required:

1. **X-Ray Daemon** — a background process that collects trace data and sends it to the X-Ray service.
    
2. **Instrumentation** — your application code must be modified (or configured) to capture tracing data using the **X-Ray SDK**.

### **Step-by-step process**
step1 : add permissions `xray:PutTraceSegments` and `xray:PutTelemetryRecords` to IAM role assigned to EC2 instance.

step 1 :. **Install and run the X-Ray Daemon** on the EC2 instance.
    - The daemon listens on UDP port `2000` for trace data.
    - It batches and sends that data to the AWS X-Ray service.



