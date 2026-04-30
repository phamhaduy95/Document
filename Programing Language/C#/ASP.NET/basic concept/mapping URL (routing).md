# Routing in ASP.NET Core

Routing is the process of mapping an incoming HTTP request to a specific executable handler, known as an **Endpoint**.

## 1. How Routing Works
In ASP.NET Core, routing is managed by two main middleware components working in tandem:

- **Routing Middleware (`UseRouting`)**: Analyzes the incoming URL and headers to determine which registered endpoint is the best match.
- **Endpoint Middleware (`UseEndpoints` or `Map...`)**: Executes the actual handler (such as a Controller action or Razor Page) that was selected by the Routing Middleware.

> [!NOTE]
> If a request URL does not match any registered route template, the request continues through the pipeline. If it reaches the end without being handled, the server typically returns a **404 Not Found** response.

---

## 2. Route Templates
A route template defines a pattern for URLs in your application. It allows a single handler to respond to a range of similar URLs.

### Template Syntax
- **Literal Value**: `/shop/products` (Matches the exact string).
- **Variable Parameter**: `/{category}` (Matches any value and stores it in the route data).
- **Optional Parameter**: `/{id?}` (The segment is not required for a match).
- **Default Value**: `/{controller=Home}` (Uses "Home" if the segment is omitted).
- **Constraints**: `/{id:int}` (Only matches if the segment can be parsed as an integer).

### Examples
| Template | Example URL | Extracted Route Values |
| :--- | :--- | :--- |
| `product/{id:int}` | `/product/123` | `id = 123` |
| `blog/{category}/{name}` | `/blog/tech/ai` | `category = "tech"`, `name = "ai"` |
| `search/{term=all}` | `/search` | `term = "all"` |

> [!WARNING]
> Do not use route constraints (like `:int` or `:min(18)`) for high-level data validation. Constraints are intended to help the router distinguish between two similar routes; actual data validation should occur during **Model Binding**.

---

## 3. Conventional vs. Attribute Routing

### Conventional Routing
Commonly used in MVC applications with Views. Routes are defined globally in a central location, usually in `Program.cs`.

```csharp
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");
```

### Attribute Routing
The preferred approach for **RESTful APIs**. Routes are defined directly on the Controller class or its Action methods using attributes.

```csharp
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")] // Matches GET api/products/5
    public IActionResult GetById(int id) 
    { 
        return Ok(); 
    }
}
```

---

## 4. Design Guidelines
- **Case Insensitivity**: Routing is not case-sensitive by default (`/Home` matches `/home`).
- **Reserved Words**: Certain words like `action`, `controller`, and `page` have special meanings when used as variables in a template.
- **Order of Precedence**: In conventional routing, the order in which you call `MapControllerRoute` matters (first match wins). In attribute routing, the system uses a scoring system based on complexity to find the best match.
