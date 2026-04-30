# JWT Bearer Authentication in ASP.NET Core

JSON Web Token (JWT) is an open standard for securely transmitting information between parties as a JSON object. It is the preferred method for authenticating users in modern APIs and Single Page Applications (SPAs) where traditional cookies are not suitable.

---

## 1. What is a JWT?
A JWT is a string consisting of three parts separated by dots (`.`):

1.  **Header**: Contains metadata about the token type (JWT) and the hashing algorithm used (e.g., HMAC SHA256).
2.  **Payload**: Contains the **Claims**—actual statements about the user (e.g., Name, Email, Roles) and token metadata (e.g., Expiration time).
3.  **Signature**: A hash of the encoded header, payload, and a server-side secret key. This prevents tampering; if the payload is changed, the signature will no longer match.

---

## 2. Configuring JWT in Program.cs
To use JWT, you must install the **`Microsoft.AspNetCore.Authentication.JwtBearer`** NuGet package.

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;
using Microsoft.IdentityModel.Tokens;
using System.Text;

builder.Services.AddAuthentication(options => {
    options.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddJwtBearer(options =>
{
    options.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuer = true,
        ValidateAudience = true,
        ValidateLifetime = true,
        ValidateIssuerSigningKey = true,
        ValidIssuer = builder.Configuration["Jwt:Issuer"],
        ValidAudience = builder.Configuration["Jwt:Audience"],
        IssuerSigningKey = new SymmetricSecurityKey(
            Encoding.UTF8.GetBytes(builder.Configuration["Jwt:Key"]))
    };
});
```

---

## 3. Issuing a Token (Login Action)
When a user provides valid credentials, the server creates a token and returns it to the client.

```csharp
public string GenerateJwtToken(IdentityUser user)
{
    var claims = new List<Claim>
    {
        new Claim(JwtRegisteredClaimNames.Sub, user.Email),
        new Claim(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
        new Claim(ClaimTypes.NameIdentifier, user.Id),
    };

    var key = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_config["Jwt:Key"]));
    var creds = new SigningCredentials(key, SecurityAlgorithms.HmacSha256);

    var token = new JwtSecurityToken(
        issuer: _config["Jwt:Issuer"],
        audience: _config["Jwt:Audience"],
        claims: claims,
        expires: DateTime.UtcNow.AddDays(7),
        signingCredentials: creds
    );

    return new JwtSecurityTokenHandler().WriteToken(token);
}
```

---

## 4. Consuming the Token
The client must include the token in the **Authorization** header for every subsequent request.

```http
Authorization: Bearer <your_jwt_token_here>
```

---

## 5. Protecting Endpoints
Once configured, use the **`[Authorize]`** attribute to protect your API controllers.

```csharp
[Authorize] // Requires a valid JWT Bearer token
[ApiController]
[Route("api/[controller]")]
public class DataController : ControllerBase
{
    [HttpGet]
    public IActionResult GetPrivateData() => Ok("Secret content");
}
```

> [!WARNING]
> Never store sensitive information like passwords in the JWT payload, as the header and payload are only Base64 encoded and can be easily read by anyone who has the token. The signature only ensures **integrity**, not **secrecy**.
