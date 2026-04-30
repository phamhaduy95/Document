# Enabling Cross-Origin Resource Sharing (CORS)

Browser security prevents a web page from making requests to a different domain than the one that served it. This restriction is known as the **Same-Origin Policy**. **CORS** (Cross-Origin Resource Sharing) is a W3C standard that allows a server to relax this policy and explicitly permit specific cross-origin requests.

---

## 1. How CORS Works
CORS is **not** a security feature—it is a mechanism that allows a server to bypass a security restriction in the browser. It works by having the server send specific HTTP headers (like `Access-Control-Allow-Origin`) that tell the browser which external domains are allowed to access the data.

---

## 2. Configuring CORS in ASP.NET Core
To enable CORS, you must define a policy in the service container and then apply it in the middleware pipeline.

### Step 1: Define a Named Policy
In `Program.cs`, register the CORS services and define your rules.

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowSpecificOrigin",
        builder =>
        {
            builder.WithOrigins("https://my-frontend-app.com", "http://localhost:3000")
                   .AllowAnyHeader()
                   .AllowAnyMethod();
        });
});
```

### Step 2: Apply the Middleware
Add the `UseCors` middleware to your pipeline. It must be placed after `UseRouting` but before `UseAuthorization`.

```csharp
app.UseRouting();

// Apply the CORS policy globally
app.UseCors("AllowSpecificOrigin");

app.UseAuthorization();
```

---

## 3. Applying CORS to Specific Endpoints
Instead of enabling CORS for the whole application, you can apply it to specific Controllers or Actions using the **`[EnableCors]`** attribute.

```csharp
[ApiController]
[Route("api/[controller]")]
[EnableCors("AllowSpecificOrigin")] // Applies only to this controller
public class ProductsController : ControllerBase
{
    [HttpGet]
    public IActionResult Get() => Ok();
}
```

---

## 4. Policy Configuration Options
- **`WithOrigins`**: Specifies the exact domains allowed to make requests.
- **`AllowAnyOrigin`**: Allows requests from any website (use with caution).
- **`WithMethods`**: Restricts which HTTP verbs (GET, POST, etc.) are allowed.
- **`WithHeaders`**: Specifies which custom headers the client is allowed to send.
- **`AllowCredentials`**: Allows the browser to send cookies or authentication tokens with the request.

> [!WARNING]
> Most browsers will block a CORS request if you use **`.AllowAnyOrigin()`** in combination with **`.AllowCredentials()`**. For authenticated requests, you must specify the exact origins.
