 Amazon Kinesis is a collection of services that let you collect, process, store, and deliver  
streaming data. Kinesis can perform real-time ingestion of gigabytes per second from thousands of sources, making it perfect for things like audio and video feeds, application logs,  
and telemetry data
AWS offers the following Kinesis services for different types of streaming data:  
■ Kinesis Video Streams  
■ Kinesis Data Streams  
■ Kinesis Data Firehose

trung gian giữa producer và consumer 

Vai trò thực sự của Kinesis trong big data workflow
dùng agent đẹ

Kinesis Video Streams  
Kinesis cho dữ liệu video phù hợp cho ứng dụng streaming
dùng timestamp làm index cho từng giữ liệu video entry
store binary data 

Kinesis thuần data, application logs, social media feeds, location tracking

have time-indexed data, such as video or radar images, Kinesis Video  
Streams is probably the best choice.
Kinesis Data Streams are indexed by the partition key and sequence number

Sự khác nhau giửa SQS và data stream
You might notice that Kinesis Data Streams seems similar to Simple  
Queue Service (SQS). SQS is typically used to allow an application component to pass small, short-lived messages to other components. SQS  
temporarily holds a small message in queue until a single consumer  
processes and deletes it. Kinesis Data Streams, on the other hand,  
is designed to provide durable storage and playback of large data  
streams—such as log files—to multiple consumers.

SQS is designed to temporarily hold a  
small message until a single consumer processes it

shards
The maximum throughput of a stream depends on the number of shards you configure.  
Each shard uniquely identifies a sequence of data records and has a fixed capacity. Each  
shard supports up to five read transactions per second, with a maximum data rate of 2 MB  
per second. For writes, you can push up to 1,000 records per second, with a data rate of  
1 MB per second. If you need more capacity, you can increase the number of shards


Kinesis Data Analytics enable SQL query stream data from Kinesis Data Stream