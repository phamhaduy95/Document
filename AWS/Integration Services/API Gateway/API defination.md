
Ta có thể yêu cầu API phải có  thông qua 


Example 2 — Optional Query String Parameter
``` bASH
aws apigateway put-method \
  --rest-api-id a1b2c3d4 \
  --resource-id xyz123 \
  --http-method GET \
  --authorization-type NONE \
  --request-parameters method.request.querystring.status=false

```

Take the `status` query string parameter from the API request  
and forward it to the backend integration (e.g. Lambda or HTTP endpoint).
```  BASH
aws apigateway put-integration \
  --rest-api-id a1b2c3d4 \
  --resource-id xyz123 \
  --http-method GET \
  --type AWS_PROXY \
  --integration-http-method POST \
  --uri arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:.../invocations \
  --request-parameters \
      "integration.request.querystring.status=method.request.querystring.status"
```