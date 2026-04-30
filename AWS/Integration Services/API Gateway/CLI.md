#### `CreateAuthorizer`
Creates a **custom authorizer** (or Lambda authorizer) for an existing API in API Gateway.
Requests to your API methods will now be **authenticated** using this authorizer 

Example (JWT Authorizer):
``` bash
aws apigateway create-authorizer \
  --rest-api-id a1b2c3d4 \
  --name MyJWTAuthorizer \
  --type JWT \
  --identity-source "method.request.header.Authorization" \
  --jwt-configuration '{"issuer":"https://example.auth0.com/","audience":["abc123"]}'

```

#### `CreateResource`
Creates a **new resource path** (i.e., a new endpoint) under an existing API.

Each “resource” in API Gateway represents a **URI path segment** (e.g., `/users`, `/orders/{id}`).

``` bash
aws apigateway create-resource \
  --rest-api-id a1b2c3d4 \
  --parent-id abcd1234 \
  --path-part users

```
#### `CreateBasePathMapping`

A **Base Path Mapping** tells **API Gateway** how to route traffic coming from a **custom domain** (like `api.example.com`) to a specific **API** and **stage** (like `myapi` → `prod`).

``` bash
aws apigateway create-base-path-mapping \
  --domain-name "api.example.com" \
  --rest-api-id "a1b2c3d4" \
  --stage "prod" \
  --base-path "v1"
```

#### `PutMethod`
Creates or updates an **HTTP method** (like `GET`, `POST`, `DELETE`, etc.) for a given resource path in your API.

It defines:
- What kind of HTTP method the API supports.
- What authorization is required.
- Whether an API key is needed.
- What request parameters or models apply.

``` bash
aws apigateway put-method \
  --rest-api-id a1b2c3d4 \
  --resource-id r123abc \
  --http-method GET \
  --authorization-type NONE \
  --api-key-required false
```


#### `PutIntegration`