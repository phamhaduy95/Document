#### API key

An **API key** is a **unique alphanumeric string** (like `123abc-456def-789ghi`) that clients include in their requests so API Gateway can:

- Identify **who** is calling your API.
- Enforce **usage limits** (throttling and quotas).
- Optionally track **usage metrics** per key.
    

An API key is **not an authentication mechanism** (it doesn’t secure the API by itself).  
It’s primarily for **rate limiting** and **monitoring**.

``` bash
aws apigateway create-api-key \
  --name "GoldCustomerKey" \
  --enabled true
```


client include in request header
``` 
x-api-key: a1b2c3d4e5f6g7h8i9j0
```


Usage Plan
A **usage plan** defines:

- Which **API stages** the plan applies to (e.g., `prod`, `dev`).
- How much **traffic** (requests per second or total requests per day/month) is allowed.
- Which **API keys** are subscribed to it.

``` bash
aws apigateway create-usage-plan \
  --name "GoldPlan" \
  --throttle burstLimit=20,rateLimit=10 \
  --quota limit=10000,period=DAY \
  --api-stages apiId=a1b2c3d4,stage=prod

```

|Setting|Meaning|
|---|---|
|`rateLimit=10`|Average 10 requests/second allowed|
|`burstLimit=20`|Max spike of 20 requests at once|
|`quota limit=10000`|10,000 requests per day|
|`api-stages`|Which API and stage are governed by this plan|

To link API key with usage plan

``` bash
aws apigateway create-usage-plan-key \
  --usage-plan-id up123abc \
  --key-id abc123 \
  --key-type API_KEY

```


Enabling API Key Requirement on a Method

``` bash
aws apigateway put-method \
  --rest-api-id a1b2c3d4 \
  --resource-id xyz123 \
  --http-method GET \
  --authorization-type NONE \
  --api-key-required true

```


nếu user gọi API mà không có API key sẽ trả về 403 