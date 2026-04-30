# Authorization in ASP.NET Core

Authorization is the process of determining whether a user has the necessary permissions to access a specific resource or endpoint. It occurs **after** authentication has established the user's identity.

---

## 1. The Authorize Attribute
The most common way to enforce authorization is via the **`[Authorize]`** attribute.

- **Basic Usage**: `[Authorize]` applied to a class or method ensures that only authenticated users can access it.
- **Bypassing Authorization**: Use **`[AllowAnonymous]`** to make specific endpoints public within an authorized controller.

```csharp
[Authorize]
[ApiController]
[Route("api/[controller]")]
public class OrdersController : ControllerBase
{
    [HttpGet] // Secured: Requires authentication
    public IActionResult GetAll() => Ok();

    [AllowAnonymous]
    [HttpGet("public-info")] // Public: No authentication required
    public IActionResult GetPublicInfo() => Ok();
}
```

---

## 2. Policy-Based Authorization
Policies allow you to group multiple requirements into a single named rule. This is much more flexible than simple role-based checks.

### Defining Policies in Program.cs
```csharp
builder.Services.AddAuthorization(options =>
{
    // Simple claim-based policy
    options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    
    // Multi-requirement policy
    options.AddPolicy("VipCustomer", policy => 
        policy.RequireAuthenticatedUser()
              .RequireClaim("Membership", "Gold", "Platinum")
              .RequireRole("Customer"));
});
```

### Applying a Policy
```csharp
[Authorize(Policy = "VipCustomer")]
public IActionResult GetExclusiveContent() => Ok();
```

---

## 3. Custom Requirements and Handlers
For complex business logic (e.g., checking if a user's age is greater than a specific value), you can implement custom requirements.

### 1. The Requirement Class
```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; }
    public MinimumAgeRequirement(int age) => MinimumAge = age;
}
```

### 2. The Authorization Handler
```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(
        AuthorizationHandlerContext context, 
        MinimumAgeRequirement requirement)
    {
        var ageClaim = context.User.FindFirst(c => c.Type == "Age");
        if (ageClaim != null && int.TryParse(ageClaim.Value, out var age))
        {
            if (age >= requirement.MinimumAge)
            {
                context.Succeed(requirement); // Mark requirement as met
            }
        }
        return Task.CompletedTask;
    }
}
```

### 3. Registration
```csharp
builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("AdultsOnly", policy => 
        policy.AddRequirements(new MinimumAgeRequirement(18)));
});

// Register the handler in the DI container
builder.Services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
```

---

## 4. Authorization Results
If a user fails authorization, the system returns one of two statuses:
- **401 Unauthorized**: The user is not logged in (Challenge).
- **403 Forbidden**: The user is logged in but lacks the required policy/claims (Forbid).

> [!IMPORTANT]
> Middleware Order matters: Always place **`app.UseAuthorization()`** after `app.UseRouting()` and `app.UseAuthentication()`, but before any endpoint mappings.
