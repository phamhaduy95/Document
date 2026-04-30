# Custom Middleware in ASP.NET Core

Middleware is software that is assembled into an application pipeline to handle requests and responses. Each component chooses whether to pass the request to the next component in the pipeline.

---

## 1. Using the `Run` Extension
The `Run` extension method is used to add a terminal middleware component to the pipeline. Because it does not call the next middleware, it should always be placed at the end of the pipeline.

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.Run(async context =>
{
    await context.Response.WriteAsync("Hello from the terminal middleware!");
});

app.Run();
```

---

## 2. Using the `Use` Extension
The `Use` extension allows you to perform logic both before and after the next middleware in the pipeline.

```csharp
app.Use(async (context, next) =>
{
    // Logic before the next middleware
    Console.WriteLine("Request passing through...");

    await next(); // Call the next middleware

    // Logic after the next middleware
    Console.WriteLine("Response passing back...");
});
```

> [!WARNING]
> Do not modify the `Response` object after calling `next()`. Subsequent middleware (like the static file middleware or MVC) may have already started streaming the response to the client.

---

## 3. Building a Custom Middleware Class
For more complex logic, you can create a dedicated middleware class. This makes your code cleaner and allows for better dependency management.

### Dependency Injection in Middleware
- **Constructor Injection**: Only for **Singleton** services. Middleware is instantiated once when the application starts, so it remains for the app's lifetime.
- **Method Injection (`Invoke` or `InvokeAsync`)**: For **Scoped** or **Transient** services. This allows you to access services that are created per-request.

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ISingletonService _singleton;

    public CustomMiddleware(RequestDelegate next, ISingletonService singleton)
    {
        _next = next;
        _singleton = singleton;
    }

    public async Task InvokeAsync(HttpContext context, IScopedService scoped)
    {
        // Perform logic using the scoped service
        scoped.DoWork();
        
        await _next(context);
    }
}
```

### Registration
Register your custom middleware in `Program.cs`:

```csharp
app.UseMiddleware<CustomMiddleware>();
```
