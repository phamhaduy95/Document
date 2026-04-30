# HttpClient and IHttpClientFactory in ASP.NET Core

The `HttpClient` class is the primary way to send HTTP requests and receive responses from web APIs. In ASP.NET Core, it is best practice to use **`IHttpClientFactory`** to manage these connections efficiently.

---

## 1. The Basics of HttpClient
`HttpClient` provides methods like `GetAsync`, `PostAsync`, `PutAsync`, and `DeleteAsync` to interact with external services.

```csharp
public async Task<string> FetchExternalData()
{
    using var client = new HttpClient();
    var response = await client.GetAsync("https://api.exchangerates.io/latest");
    
    // Throws an exception if the status code is not 2xx
    response.EnsureSuccessStatusCode(); 
    
    return await response.Content.ReadAsStringAsync();
}
```

---

## 2. Why Use IHttpClientFactory?
While `HttpClient` is `IDisposable`, instantiating it with `using` for every request can lead to **Socket Exhaustion**. The system ports are not immediately released, which can crash high-traffic applications.

**`IHttpClientFactory` solves this by:**
- **Managing Handler Lifetimes**: It pools the underlying `HttpMessageHandler` objects to avoid socket issues.
- **Respecting DNS Changes**: It periodically refreshes connections to handle DNS updates.
- **Centralized Configuration**: Allows you to configure clients once in `Program.cs`.

---

## 3. Implementation Patterns

### Named Clients
Register a specific configuration with a name in `Program.cs`.
```csharp
builder.Services.AddHttpClient("GitHub", client =>
{
    client.BaseAddress = new Uri("https://api.github.com/");
    client.DefaultRequestHeaders.Add("User-Agent", "MyDotNetApp");
});
```

Consume the named client via injection:
```csharp
public class MyService
{
    private readonly IHttpClientFactory _factory;
    public MyService(IHttpClientFactory factory) => _factory = factory;

    public async Task GetData()
    {
        var client = _factory.CreateClient("GitHub");
        var result = await client.GetAsync("repos/dotnet/aspnetcore");
    }
}
```

### Typed Clients
The recommended pattern for clean code. It provides type safety and encapsulates API logic.
```csharp
// Registration
builder.Services.AddHttpClient<WeatherClient>(client => {
    client.BaseAddress = new Uri("https://api.weather.com/");
});

// Class implementation
public class WeatherClient
{
    private readonly HttpClient _client;
    public WeatherClient(HttpClient client) => _client = client;

    public async Task<WeatherInfo> GetWeatherAsync() => 
        await _client.GetFromJsonAsync<WeatherInfo>("today");
}
```

---

## 4. Working with JSON
Modern .NET provides extension methods in `System.Net.Http.Json` to simplify working with JSON payloads.

- **`GetFromJsonAsync<T>`**: Sends a GET request and deserializes the JSON body.
- **`PostAsJsonAsync<T>`**: Serializes an object to JSON and sends it in a POST request.

```csharp
var newItem = new Product { Name = "Gadget", Price = 99.99 };
var response = await client.PostAsJsonAsync("api/products", newItem);
```

> [!TIP]
> Always use `IHttpClientFactory` in production environments to ensure your application remains stable and performs well under heavy load.
