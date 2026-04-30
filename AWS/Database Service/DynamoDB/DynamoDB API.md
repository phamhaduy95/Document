#### `GetItem`
The `GetItem` operation returns a set of attributes for the item with the given primary key
important parameters
- `--table-name`: The name of your DynamoDB table.
- `--key`: A JSON object representing the primary key of the item you want to retrieve. You must provide values for all primary key attributes.

Simple Primary Key
``` bash
aws dynamodb get-item \
    --table-name Users \
    --key '{"UserID": {"S": "user123"}}'
    --projection-expression "Email, Age"
```


Composite Primary Key (Partition Key and Sort Key)
``` bash
aws dynamodb get-item \
    --table-name MusicCollection \
    --key '{"Artist": {"S": "No One You Know"}, "SongTitle": {"S": "Call Me Today"}}'

```
#### `PutItem`
``` bash
aws dynamodb put-item \
    --table-name ProductCatalog \
    --item file://item.json \
    --condition-expression "attribute_not_exists(Id)"
```

#### `TransactGetItem`
To retrieve multiple items atomically from one or more DynamoDB tables using the AWS CLI,
A single operation can retrieve up to 16 MB of data, which can contain as many as 100 items.
limit 100 items:
When designing your application, keep in mind that DynamoDB does not return items in any particular order

``` bash
aws dynamodb transact-get-items \
    --transact-items file://items-to-get.json
```

``` JSON
[
    {
        "Get": {
            "TableName": "Products",
            "Key": {
                "ProductID": {"S": "PROD123"},
                "SK": {"S": "Details"}
            },
            "ProjectionExpression": "ProductName, Price"
        }
    },
    {
        "Get": {
            "TableName": "Orders",
            "Key": {
                "OrderID": {"S": "ORDER456"}
            }
        }
    }
]
```
#### `TransactWriteItems`
``` bash 
aws dynamodb transact-write-items \
    --transact-items file://items-to-write.json
```


``` JSON
[
    {
        "ConditionCheck": {
            "TableName": "BankingTable",
            "Key": {
                "AccountID": {"S": "ACC001"}
            },
            "ConditionExpression": "Balance >= :amount_to_send",
            "ExpressionAttributeValues": {
                ":amount_to_send": {"N": "50"}
            }
        }
    },
    {
        "Update": {
            "TableName": "BankingTable",
            "Key": {
                "AccountID": {"S": "ACC001"}
            },
            "UpdateExpression": "SET Balance = Balance - :amount",
            "ExpressionAttributeValues": {
                ":amount": {"N": "50"}
            }
        }
    },
    {
        "Update": {
            "TableName": "BankingTable",
            "Key": {
                "AccountID": {"S": "ACC002"}
            },
            "UpdateExpression": "SET Balance = Balance + :amount",
            "ExpressionAttributeValues": {
                ":amount": {"N": "50"}
            }
        }
    }
]
```

The `ConditionCheck` specifies a requirement that must be true for the transaction to succeed: 
throw `TransactionCanceledException` error khi `ConditionCheck` fail 
#### Query
You can use the `Query` API operation in Amazon DynamoDB to find items based on primary key values.
You must provide the name of the partition key attribute and a single value for that attribute. `Query` returns all items with that partition key value

`Query` results are always sorted by the sort key value

Example: `orderID` is sort key và `CustomerID` là partition key
``` bash
aws dynamodb query \
    --table-name OrdersTable \
    --key-condition-expression "CustomerID = :c_id AND #orderId > :order_id" \
    --filter-expression "Amount > :amt" \
    --expression-attribute-names '{"#orderId": "OrderID}' \
    --expression-attribute-values '{":c_id": {"S": "cust123"}, ":order_id": {"S": "order001"}}'

```


`--filter-expression`: mô tả logic filter kết quả trả về. Quá trình filter diễn ra sau khi data được query từ database về client 
#### `UpdateTimeToLive`

To retrieve multiple items atomically from one or more DynamoDB tables using the AWS CLI, you use the `aws dynamodb transact-get-items` command.

The following command enables TTL on the `MusicCollection` table, using the attribute named `Expiration` to store the item's expiration timestamp

```bash
aws dynamodb transact-get-items \
    --transact-items file://items-to-get.json
```

#### `UpdateTable`
You can only perform one of the following operations at once:

- Modify the provisioned throughput settings of the table.
- Remove a global secondary index from the table.
- Create a new global secondary index on the table. After the index begins backfilling, you can use `UpdateTable` to perform other operations

**[ReturnConsumedCapacity](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_BatchGetItem.html#API_BatchGetItem_RequestSyntax)**

Determines the level of detail about either provisioned or on-demand throughput consumption that is returned in the response:

- `INDEXES` - The response includes the aggregate `ConsumedCapacity` for the operation, together with `ConsumedCapacity` for each table and secondary index that was accessed.
    
    Note that some operations, such as `GetItem` and `BatchGetItem`, do not access any indexes at all. In these cases, specifying `INDEXES` will only return `ConsumedCapacity` information for table(s).
- `TOTAL` - The response includes only the aggregate `ConsumedCapacity` for the operation.
- `NONE` - No `ConsumedCapacity` details are included in the response.