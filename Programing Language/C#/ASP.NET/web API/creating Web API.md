# Creating Web APIs in ASP.NET Core

ASP.NET Core Web APIs provide a powerful framework for building RESTful services that can be consumed by mobile apps, browsers, and other server-side applications. They share the same core engine as MVC but focus on returning machine-readable data (JSON/XML) rather than HTML views.

---

## 1. Setting Up a Web API Project
A modern Web API project is configured in `Program.cs` to handle attribute routing and provide API documentation.

```csharp
var builder = WebApplication.CreateBuilder(args);

// Register API services
builder.Services.AddControllers(); 
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen(); // Generates OpenAPI documentation

var app = builder.Build();

// Enable Swagger UI for easy testing in Development
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers(); // Maps routes based on [Route] attributes

app.Run();
```

---

## 2. The API Controller
Web API controllers should inherit from **`ControllerBase`** and include the **`[ApiController]`** attribute.

```csharp
[ApiController]
[Route("api/[controller]")] // Automatically uses the class name (e.g., api/Students)
public class StudentsController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetStudent(int id)
    {
        // Ok() returns a 200 status code with the data
        return Ok(new { Id = id, Name = "Alice" });
    }
}
```

### Important Features:
- **`[ApiController]`**: Enables automatic 400 Bad Request responses for validation errors and simplifies data binding.
- **`ControllerBase`**: A base class for controllers without view support, providing methods like `Ok()`, `NotFound()`, and `CreatedAtAction()`.
- **Token Replacement**: Using `[controller]` and `[action]` in routes helps keep URLs consistent even if you rename your classes.

---

## 3. Handling HTTP Verbs
RESTful APIs use HTTP methods to define the nature of the operation:

| Attribute | HTTP Verb | Purpose |
| :--- | :--- | :--- |
| **`[HttpGet]`** | GET | Read or retrieve data. |
| **`[HttpPost]`** | POST | Create new resources. |
| **`[HttpPut]`** | PUT | Update an existing resource. |
| **`[HttpDelete]`** | DELETE | Remove a resource. |

---

## 4. Action Return Types
For maximum flexibility, use **`ActionResult<T>`** as the return type for your actions. It allows you to return either a specific object or an HTTP status code helper.

```csharp
[HttpGet("{id}")]
public ActionResult<Product> GetProduct(int id)
{
    var product = _repository.Get(id);
    
    if (product == null)
    {
        return NotFound(); // Returns 404
    }
    
    return product; // Returns 200 OK with the product JSON
}
```

---

## 5. Testing with Swagger (OpenAPI)
ASP.NET Core includes built-in support for Swagger. When running your app locally, navigate to `/swagger` to access a web-based testing interface. This allows you to explore every endpoint, see the expected request structure, and execute real requests directly from your browser.
