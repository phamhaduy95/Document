
#### Step to deploy Serverless Application Model 

**Step 1:** Init project

**Step 2: Build the app**
``` bash
sam build
```

**Step 3: Deploy** 

``` bash
`sam deploy --guided`
```

#### Testing lambda locally with SAM
Ta có thể testing lambda thông qua các cách sau:
- sử dụng `sam local invoke` để invoke lambda function locally với custom input
- dùng `sam local start-lambda` start local endpoint mô phỏng các event từ AWS service như S3
- dùng `sam local start-api` trigger 1 API endpoint của API gateway để test integrate APIs




