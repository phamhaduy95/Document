
_AWS Security Token Service (AWS STS)_ creates temporary security credentials and provides trusted users with those temporary security credentials. The trusted users then access AWS resources with those credentials. Temporary security credentials work similarly to long-term access key credentials, but with the following differences:

#### Credential Expiration

| Source                      | Default Expiration    | Notes                               |
| --------------------------- | --------------------- | ----------------------------------- |
| `AssumeRole`                | 1 hour (max 12 hours) | Default 1h, configurable            |
| `GetSessionToken`           | 12 hours              | Up to 36 hours for root credentials |
| `AssumeRoleWithWebIdentity` | 1 hour                | Fixed                               |
| `GetFederationToken`        | 12 hours              | Legacy API                          |
#### STS in Cognito (Identity Pools)

When a user signs in via Cognito Identity Pool:

1. User authenticates (Cognito User Pool, Google, etc.)
2. Cognito calls `AssumeRoleWithWebIdentity` behind the scenes.
3. STS returns **temporary AWS credentials**.
4. User can access AWS resources (S3, DynamoDB, etc.) with those creds.

##### `AssumeRole`
`AsumeRole` cho 1 EC2 instance

``` bash
aws sts assume-role \
  --role-arn arn:aws:iam::444455556666:role/TargetRole \
  --role-session-name mySession

```

Ta có thể Assume Role trong account B vào 1 EC2 instance của account A
##### `AssumeRoleWithWebIdentity`

Used by **Cognito Identity Pools** or apps that authenticate with **OIDC tokens** (e.g., Google, Facebook, Auth0).

``` bash
aws sts assume-role-with-web-identity \
  --role-arn arn:aws:iam::123456789012:role/WebAppRole \
  --role-session-name WebIdentitySession \
  --web-identity-token file://token.jwt
```

STS exchanges the **JWT token** for temporary AWS credentials.  
This is how **Cognito Identity Pool** grants AWS access to logged-in users.
##### `GetSessionToken`

lấy temporary credential thông qua MFA serial number và token code.

``` bash
aws sts get-session-token \
  --serial-number arn:aws:iam::123456789012:mfa/DuyPham \
  --token-code 123456

```