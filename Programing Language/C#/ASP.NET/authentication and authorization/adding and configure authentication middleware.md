# Configuring Authentication Middleware

Authentication in ASP.NET Core is a modular system that identifies the user making a request. To configure it effectively, you must understand the relationship between **Schemes**, **Handlers**, and the **Middleware Pipeline**.

d

## 1. Core Concepts

### Authentication Scheme
A **Scheme** is a unique name that corresponds to a specific authentication method. 
- *Examples*: `"Cookies"`, `"Bearer"`, `"Google"`.

### Authentication Handler
A **Handler** is the engine that performs the actual authentication logic. It examines the incoming request (looking for a cookie or a JWT token) and returns an `AuthenticateResult` indicating whether the user's identity was successfully verified.

---

## 2. Registering Authentication Services
You register and configure your authentication methods in `Program.cs`. You can chain multiple methods together to support different login types.

```csharp
using Microsoft.AspNetCore.Authentication.Cookies;
using Microsoft.AspNetCore.Authentication.JwtBearer;

builder.Services.AddAuthentication(options => 
{
    // Set the default behavior for the whole app
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme;
})
.AddCookie(options => 
{
    options.LoginPath = "/Account/Login";
})
.AddJwtBearer(options => 
{
    // JWT specific configuration here
});
```

---

## 3. Forwarding Responsibilities
An authentication scheme can "forward" specific security tasks to a different scheme. This is useful for hybrid scenarios (e.g., using local cookies for session storage but external providers for login challenges).

| Option | Description |
| :--- | :--- |
| **`ForwardAuthenticate`** | Forwards the task of identifying the user. |
| **`ForwardChallenge`** | Forwards the task of prompting the user to log in (e.g., a 401 redirect). |
| **`ForwardForbid`** | Forwards the task of handling access denied (e.g., a 403 error). |

### Example: Forwarding Challenges to Google
```csharp
builder.Services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options => 
    {
        // If the user isn't logged in, don't show the local login page; send them to Google.
        options.ForwardChallenge = "Google";
    })
    .AddGoogle("Google", options => { 
        options.ClientId = "...";
        options.ClientSecret = "...";
    });
```

---

## 4. Enabling the Middleware
Registering services is not enough; you must also tell the ASP.NET Core pipeline to use them.

```csharp
app.UseRouting();

// This middleware identifies the user
app.UseAuthentication(); 

// This middleware checks if the identified user has permissions
app.UseAuthorization(); 

app.MapControllers();
```

> [!IMPORTANT]
> The placement of **`app.UseAuthentication()`** is critical. It must appear **after** `app.UseRouting()` so the system knows which endpoint is being accessed, but **before** `app.UseAuthorization()` so the user is identified before permissions are checked.


*ubiquitous language*: a shared vocabulary between software developers and domain expert. Each domain often consists of several jargon and terminologies whose meaning might be different from outside its original context. Therefore developers must actively learn those terms to prevent any confusion when communicating to other stakeholder.
