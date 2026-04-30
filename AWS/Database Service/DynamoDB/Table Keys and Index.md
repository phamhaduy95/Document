#### Table Primary Keys
Partition Key
You can **only use an equality condition (`=`)** on the partition key.
**Sort Key**: dùng làm căn cứ sort dữ liệu cho API `Query` và `Scam`. Hỗ trợ `Query` theo range
comparison operators (`<`, `>`, `<>`, `<=`, `>=`, `BETWEEN`) 

primary key bao gồm 2 loại chính: 
- **Single Primary Key**:" chỉ có Partition Key
- **Composite Primary Key:** bao gồm Partition Key và Sort Key



#### secondary index
Query và Scan sử dụng secondary index để thu thấp data từ database với combo key khác primary key.

**Local Secondary Index**:  cho phép ta query với 1 composite primary key bao gồm partition key và sort key tùy chọn. local secondary index dùng chung RCU và WCU provision với base table. 

>[!important] Quan trong
>local secondary index cần được khởi tạo trước khi table được sinh ra và không thể delete

Ví dụ: Table Forum có Partition Key là `ForumId`. Ta tạo một `Local Secondary Index` là Composition Key bao gồm Partition Key là sort key khác là `LastPostDateTime`

``` bash
 --local-secondary-indexes '[
        {
            "IndexName": "LastPostDateTimeIndex",
            "KeySchema": [
                {"AttributeName": "ForumId", "KeyType": "HASH"},
                {"AttributeName": "LastPostDateTime", "KeyType": "RANGE"}
            ],
            "Projection": {
                "ProjectionType": "ALL"
            }
        }
    ]'
```

**Global Secondary Index (GSI)**: cho phép ta tạo 1 custom key cho API query và Scan mà không cần bao gồm Partition Key. **Global Secondary Index** có provision RCU và WCU riêng so với base table. Nên sẽ có hiện tượng query dùng **GSI** bị throttle trong khi RCU của base table chưa dùng hết. 
Khi write data thì WCU của GSI và  base table đều bị consume do cần lưu dữ liệu cho cả 2 nơi.

> [!note] Lưu ý
> Để đảm bảo performance tránh hiện tượng hot partition, ta cũng cần đảm bảo custom partition key trong GSI có tính chất giống với partition key thông thường

#### Parallel Scan
 
By default, the `Scan` operation processes data sequentially. Amazon DynamoDB returns data to the application in 1 MB increments, and an application performs additional `Scan` operations to retrieve the next 1 MB of data.

The larger the table or index being scanned, the more time the `Scan` takes to complete. In addition, a sequential `Scan` might not always be able to fully use the provisioned read throughput capacity: Even though DynamoDB distributes a large table's data across multiple physical partitions, a `Scan` operation can only read one partition at a time. For this reason, the throughput of a `Scan` is constrained by the maximum throughput of a single partition.

To address these issues, the `Scan` operation can logically divide a table or secondary index into multiple _segments_, with multiple application workers scanning the segments in parallel. Each worker can be a thread (in programming languages that support multithreading) or an operating system process. To perform a parallel scan, each worker issues its own `Scan` request with the following parameters:

- `Segment` — A segment to be scanned by a particular worker. Each worker should use a different value for `Segment`.
    
- `TotalSegments` — The total number of segments for the parallel scan. This value must be the same as the number of workers that your application will use.