# Handling JSON with Newtonsoft.Json

While .NET now includes a built-in `System.Text.Json` library, **Newtonsoft.Json** (also known as Json.NET) remains one of the most popular and feature-rich libraries for JSON manipulation in the .NET ecosystem.

---

## 1. Key Features
- **Flexibility**: Handles complex object graphs, including circular references.
- **LINQ to JSON**: Allows you to query and modify JSON objects manually without needing to map them to a C# class.
- **Customization**: Offers deep control over date formats, property naming (CamelCase vs. PascalCase), and null value handling.
- **Performance**: Highly optimized for both speed and memory usage.

---

## 2. Basic Serialization and Deserialization
First, install the **`Newtonsoft.Json`** NuGet package.

### Serialization (Object to JSON string)
Use `JsonConvert.SerializeObject()` to convert a .NET object into a JSON-formatted string.

```csharp
using Newtonsoft.Json;

var product = new Product
{
    Name = "Apple",
    ExpiryDate = new DateTime(2025, 12, 31),
    Price = 3.99M,
    Sizes = new string[] { "Small", "Medium", "Large" }
};

string json = JsonConvert.SerializeObject(product, Formatting.Indented);
```

### Deserialization (JSON string to Object)
Use `JsonConvert.DeserializeObject<T>()` to convert a JSON string back into a strongly-typed .NET object.

```csharp
string jsonInput = @"{ 'Name': 'Orange', 'Price': 1.99 }";

Product product = JsonConvert.DeserializeObject<Product>(jsonInput);
```

---

## 3. Controlling Property Mapping
You can use attributes to control how specific properties are serialized or deserialized.

| Attribute | Purpose |
| :--- | :--- |
| **`[JsonProperty("name")]`** | Specifies the key name in the JSON (e.g., mapping `UserName` to `user_id`). |
| **`[JsonIgnore]`** | Excludes a property from the JSON output (useful for sensitive data). |
| **`[JsonRequired]`** | Ensures a property must exist in the JSON during deserialization. |

```csharp
public class User
{
    [JsonProperty("user_id")]
    public int Id { get; set; }

    [JsonIgnore]
    public string InternalToken { get; set; }
}
```

---

## 4. Integration with ASP.NET Core
To use Newtonsoft.Json as the default serializer in an ASP.NET Core project (replacing the default `System.Text.Json`), install the `Microsoft.AspNetCore.Mvc.NewtonsoftJson` package and update `Program.cs`:

```csharp
builder.Services.AddControllers()
    .AddNewtonsoftJson(options =>
    {
        options.SerializerSettings.NullValueHandling = Newtonsoft.Json.NullValueHandling.Ignore;
    });
```
