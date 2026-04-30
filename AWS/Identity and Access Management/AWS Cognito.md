Cognito cung cấp authentication and authorization cho web và mobile:  

- **Cognito ’s user pools** : you can add user sign-up and sign-in to your applications.  
- **Cognito ’s identity pools**:  you can give your application users temporary, controlled access to other AWS services.

#### User Pool
A **User Pool** is a _user directory_ that handles:

- **Sign-up / Sign-in**
- **Account recovery**
- **Password policies**
- **MFA (Multi-Factor Authentication)**
- **Tokens (JWTs):** ID token, Access token, Refresh token

#### Identity pool
An **Identity Pool** maps authenticated users (from User Pools or other IdPs) to **temporary AWS credentials**.

It uses **STS (Security Token Service)** to issue short-lived credentials.

When you set up an identity pool, you define the pool from which your users can come  (using Cognito, AWS, federated, or even unauthenticated identities). You then create and  assign an IAM role to the pool. Once your pool is live, any user whose identity matches your  definition will have access to the resources specified in the role.

IAM Role Mapping example
``` yaml
AuthenticatedRole: arn:aws:iam::123456789012:role/CognitoAuthRole
UnauthenticatedRole: arn:aws:iam::123456789012:role/CognitoGuestRole
```

#### developer-authenticated identities
When using **developer-authenticated identities**, you can:

- Authenticate users **with your own custom authentication system**.
    
- Then exchange your app’s authentication tokens for **temporary AWS credentials** and a **unique Cognito identity ID** via Cognito.
    

Each user gets a **consistent Cognito Identity ID** across sessions and devices, as long as they authenticate as the same user in your system.

Common Cognito Triggers (Lambda)
Cognito can invoke **Lambda triggers** during the authentication life cycle.

| Trigger                                                                     | When It Runs                | Example Use                             |
| --------------------------------------------------------------------------- | --------------------------- | --------------------------------------- |
| `PreSignUp`                                                                 | Before user signs up        | Validate email domain                   |
| `PostConfirmation`                                                          | After user confirms sign-up | Send welcome email                      |
| `PreAuthentication`                                                         | Before authentication       | Check if user blocked                   |
| `PostAuthentication`                                                        | After authentication        | Update last-login timestamp             |
| `CustomMessage`                                                             | During MFA / confirmation   | Customize SMS/email                     |
| `DefineAuthChallenge`, `CreateAuthChallenge`, `VerifyAuthChallengeResponse` | For custom auth flows       | Implement step-up or passwordless login |
