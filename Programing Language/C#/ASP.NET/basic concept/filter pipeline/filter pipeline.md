# Filter Pipeline in ASP.NET Core

Filters in ASP.NET Core allow you to run code before or after specific stages in the request processing pipeline. They are a powerful tool for handling cross-cutting concerns within MVC and Razor Pages applications.

---

## 1. Middleware vs. Filters
While both handle common logic across multiple requests, they serve different purposes:

- **Middleware**: Runs for **all** incoming requests. It operates at a "lower level" and does not have access to MVC-specific concepts like `ModelState`, `ActionContext`, or `IActionResults`.
- **Filters**: Run only for requests that reach the **Endpoint Middleware** (MVC or Razor Pages). They have access to the full MVC context and can be applied selectively to specific controllers or actions.

> [!TIP]
> Use **Middleware** for concerns that apply to the entire app (e.g., logging, static files, error pages). 
> Use **Filters** for logic that needs to interact with MVC metadata or should only apply to a subset of your actions (e.g., custom model validation, action-specific caching).

---

## 2. The Filter Pipeline Stages
The pipeline consists of several stages, each allowing you to hook into different parts of the request lifecycle:

| Filter Type | Execution Timing | Use Case |
| :--- | :--- | :--- |
| **Authorization** | First filter to run. | Check user permissions. |
| **Resource** | Runs after Auth, before Model Binding. | Caching, short-circuiting based on headers. |
| **Action** | Runs before and after the action method. | Manipulating `ModelState` or action arguments. |
| **Exception** | Runs only if an unhandled exception occurs. | Action-specific error handling or logging. |
| **Result** | Runs before and after the result execution. | Customizing the final response (e.g., adding headers). |

---

## 3. Creating and Applying Filters

### Creating a Filter
You can implement either the synchronous (e.g., `IActionFilter`) or asynchronous (`IAsyncActionFilter`) interface.

```csharp
public class MyActionFilter : IActionFilter
{
    public void OnActionExecuting(ActionExecutingContext context)
    {
        // Runs before the action method executes
    }

    public void OnActionExecuted(ActionExecutedContext context)
    {
        // Runs after the action method executes
    }
}
```

### Applying Filters
Filters can be applied at three different levels of scope:
1.  **Global Scope**: Applies to all actions in the app (configured in `Program.cs`).
2.  **Controller Scope**: Applies to all actions within a specific controller class.
3.  **Action Scope**: Applies only to a specific action method.

```csharp
// Global Registration in Program.cs
builder.Services.AddControllers(options =>
{
    options.Filters.Add<MyActionFilter>();
});

// Attribute Usage on a Controller
[ServiceFilter(typeof(MyActionFilter))]
public class HomeController : Controller { ... }
```

---

## 4. Short-Circuiting the Pipeline
Like middleware, a filter can stop the execution of the rest of the pipeline by setting the `Result` property on the context object.

```csharp
public void OnResourceExecuting(ResourceExecutingContext context)
{
    if (!IsFeatureEnabled)
    {
        // Short-circuits and returns a 400 Bad Request immediately
        context.Result = new BadRequestResult(); 
    }
}
```

---

## 5. Dependency Injection in Filters
Standard attributes cannot have constructor parameters that are resolved by DI. To use services from the DI container in your filters, use one of the following:

- **`ServiceFilter`**: Resolves the filter from the DI container (you must register the filter class as a service first).
- **`TypeFilter`**: Creates the filter instance using DI, allowing you to pass some arguments manually while others are resolved automatically.
