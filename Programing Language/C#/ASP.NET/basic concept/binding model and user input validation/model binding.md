# Model Binding in ASP.NET Core

Model binding is the process that automatically extracts data from an incoming HTTP request (from the URL, body, or headers) and maps it to action method parameters or page model properties. This eliminates the need for manual parsing of the request.

---

## 1. How Model Binding Works
When a request matches a route, the model binder scans the HTTP request for values that match the names of the parameters in your handler method.

```csharp
[HttpGet("api/users/{id}")]
public IActionResult GetUser(string id)
{
    // If the URL is api/users/0115, the variable 'id' will be bound to "0115"
    return Ok($"User data for: {id}");
}
```

---

## 2. Binding Complex Types
For requests containing multiple data fields (such as a form submission or a JSON payload), you can bind the data to a plain old CLR object (POCO).

### Requirements for Binding Classes:
- The class must be **public**.
- It must have a **public parameterless constructor**.
- Target properties must be **public** and have **public setters**.

```csharp
public class UserViewModel
{
    public Guid Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}

[HttpPost("api/students")]
public IActionResult AddStudent(UserViewModel user)
{
    // ASP.NET Core automatically instantiates and populates the 'user' object
    return Ok(user);
}
```

---

## 3. Binding Sources
By default, ASP.NET Core looks for data in the following order:
1.  **Form values**: Data sent in the body of a POST request via a form.
2.  **Route values**: Values obtained from URL segments (e.g., `{id}`).
3.  **Query string**: Key-value pairs passed at the end of the URL (e.g., `?name=test`).

### Explicit Source Attributes
You can force the model binder to look in a specific part of the request using these attributes:

| Attribute | Source |
| :--- | :--- |
| **`[FromRoute]`** | Values from the URL path. |
| **`[FromQuery]`** | Values from the URL query string. |
| **`[FromForm]`** | Values from a posted form body. |
| **`[FromBody]`** | Values from the request body (typically JSON or XML). |
| **`[FromHeader]`** | Values from specific HTTP headers. |

```csharp
[HttpPost]
public IActionResult UpdateProfile([FromBody] ProfileData data, [FromHeader("User-Agent")] string browser)
{
    // 'data' comes from the JSON body, 'browser' comes from the header
    return Ok();
}
```

---

## 4. Handling Missing Data
If no matching value is found for a parameter or property:
- **Nullable types**: Set to `null`.
- **Non-nullable value types**: Set to their default value (e.g., `0` for `int`, `false` for `bool`).
- **Complex types**: An instance is created using the default constructor, but its properties are not populated.
- **Arrays**: Set to `Array.Empty<T>()`, except for `byte[]` which is set to `null`.

> [!TIP]
> To ensure that data is not missing and is valid, you should use **Model Validation** attributes like `[Required]` or `[StringLength]`.
