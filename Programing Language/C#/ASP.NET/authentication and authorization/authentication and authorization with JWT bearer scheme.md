**Authentication and Authorization with JWT Bearer**

1.  **Understand JWT bearer**

**1.1 Introduction**

In the modern age of internet, server does not only serve data for
client in browser but across many kinds of clients and devices. Relying
on authentication cookie is not suitable as it is only support in
browser client. JSON Web Token or JWT arises as the standard approach
for handling authentication and authorization for a web app. The diagram
below illustrates how the JWT is generated and works.

![](media/image1.png){width="6.520574146981628in"
height="4.28307852143482in"}

The client sends the request for authentication which may consists of
sign in data such as username or email and most importantly password.

The server then validates user login data if it finds the login request
is valid then it send back the HTTP response which includes a token.

Then client will keep that token and inject its in the header
(specifically the *authorization* header) of any further request.

**1.2 JWT format**

WT is represented as a combination of three base64url encoded parts
concatenated with period (\'.\') characters and comprises the following
three sections:

- Header

- Payload

- Signature

**Header Section**

This section provides metadata about the type of data and the algorithm
to be used to encrypt the data that is to be transferred. Two properties
"*typ*" and "*alg*" are used to indicate its type of signature and which
encryption algorithm is used for encoding and decoding the token

> {
>
> \"*typ*\": \"JWT\",
>
> \"*alg*\": \"HS256\"
>
> }

**Payload**

The payload represents the actual information in JSON format that is to
be transmitted over the wire.

The payload typically may contain claims, the identity information of
the user, the allowed permissions. The payload uses properties as the
reserved ones.

- *iss:* This represents the issuer of the token.

- *sub:* This is the subject of the token.

- *aud*: This represents the audience of the token.

- *exp*: This is used to define token expiration.

- *nbf*: This is used to specify the time before which the token must
  not be processed.

- *iat*: This represents the time when the token was issued.

- *jti:* This represents a unique identifier for the token.

You can also use custom claims, which can be added to the token using a
rule. The code snippet given below illustrates a simple payload.

> {
>
> \"*sub*\": \"*1234567890*\",
>
> \"*name*\": \"*Joydip* *Kanjilal*\",
>
> \"*admin*\": true,
>
> \"*jti*\": \"*cdafc246-109d-4ac9-9aa1-eb689fad9357*\",
>
> \"*iat*\": 1611497332,
>
> \"*exp*\": 1611500932
>
> }

**Signature**

The signature adheres to the JSON Web Signature (JWS) specification and
is used to verify the integrity of the data transferred over the wire.
It comprises a hash of the header, the payload, and the secret, and is
used to ensure that the message was not changed while being transmitted.
The final signed token is created by adhering to the JSON Web Signature
(JWS) specification. The encoded JWT header and as well as the encoded
JWT payload is combined and then it\'s signed using a strong encryption
algorithm such as HMAC SHA 256.

2.  **JWT bearer scheme in ASP.NET core**

To integrate JWT bearer authentication in your project, install NUGET
packet Microsoft.AspNetCore.Authentication.JwtBearer. Then in your main
entry program.cs, add JWT authentication scheme through authentication
middleware configuration.

builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme).AddJwtBearer(JwtBearerDefaults.AuthenticationScheme
,options =\>

{

option.RequireHttpsMetadata = true;

option.SaveToken = true;

option.Author = ""

option.TokenValidationParameters = new TokenValidationParameters()

{

ValidateIssuer = true,

ValidateAudience = true,

ValidateLifetime = true,

ValidateIssuerSigningKey = true,

ValidAudience = builder.Configuration\[\"Jwt:Audience\"\],

ValidIssuer = builder.Configuration\[\"Jwt:Issuer\"\],

IssuerSigningKey = new
SymmetricSecurityKey(Encoding.UTF8.GetBytes(builder.Configuration\[\"Jwt:Key\"\]))

};

});

You can add JWTBearer scheme as the default scheme for authentication,
and use AddJWTBearer method for configuring this scheme further through
options pattern. There some noticeable options you should consider:

- RequireHttpsMetadata: when set is true will requires the token
  transferred via HTTPs for more strong security. In the deployment
  setting, this option must be set to true.

- Audience: represents the intended recipient of the incoming token or
  the resource that the token grants access to. If the value specified
  in this parameter doesn't match the aud parameter in the token, the
  token will be rejected because it was meant to be used for accessing a
  different resource. you can specific this inside the
  TokenValidationParameters object.

- SaveToken: Allow to save the token inside the
  AuthenticationProperties, which can be retrieved from elsewhere within
  your app through HttpContext.GetTokenAsync().

- Author: is the address of the token-issuing authentication server. The
  JWT bearer authentication middleware will use this URI to find and
  retrieve the public key that can be used to validate the token's
  signature. For example: option.Authority = \"http://localhost:5000/\";

- TokenValidationParemeters option must be always provided when the
  author options are not given, as the ASP.NET will use it to specify
  more advanced options for how JWT tokens will be validated which can
  normally be found in token-issuing server. Like the example above, the
  issuer, audience and expired time are set required for validation. We
  also provide the list of valid value for issuer, audience and most
  importantly the proper key for decoding the token.

**Creating API for granting Token to user**

In the first section, we did discuss about the token that return when
user try to sign in to your web app. The sample code below tells us some
detail step for composing the appropriate token based on all options we
add in the authentication middleware.

public async Task\<LoginResult\> Login(LoginModel model) {

var user = await \_userManager.FindByEmailAsync(model.Email);

if (user == null) return LoginResult.GetFailResult(\"user not found\");

var result = await \_signInManager.PasswordSignInAsync(user,
model.Password, false, false);

if (!result.Succeeded) return LoginResult.GetFailResult(\"wrong
password\");

List\<Claim\> claims = new List\<Claim\>()

{

new Claim(\"userId\",user.Id.ToString()),

new Claim(ClaimTypes.Name, user.UserName),

new Claim(\"PhoneNumber\", user.PhoneNumber),

new Claim(ClaimTypes.Email, user.Email),

};

var claimIdentity = new ClaimsIdentity(claims);

var key = new
SymmetricSecurityKey(Encoding.UTF8.GetBytes(\_configuration\[\"Jwt:Key\"\]));

var signIn = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

var token = new JwtSecurityToken

(

\_configuration\[\"Jwt:Issuer\"\],

\_configuration\[\"Jwt:Audience\"\],

claimIdentity.Claims,

expires: DateTime.UtcNow.AddDays(1),

signingCredentials: signIn);

string strToken = new JwtSecurityTokenHandler().WriteToken(token);

return LoginResult.GetSuccessfulResult(strToken);

}

**Add authorization for JWT bearer.**

To make ASP.NET authorize any endpoint using JWT bearer scheme,
