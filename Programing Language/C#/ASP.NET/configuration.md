# Configuration in ASP.NET Core

Configuration in ASP.NET Core is performed using various configuration providers. These providers read configuration data from key-value pairs using several sources.

---

## 1. What is Configuration?
Configuration is the set of external parameters that control an application's behavior.
- **Settings**: Values that change how the app behaves.
- **Secrets**: Sensitive data like passwords, API keys, or connection strings.

Storing these in external files (like `appsettings.json`) avoids unnecessary recompilations and helps keep secrets out of source control.

---

## 2. Configuration Providers
ASP.NET Core loads configuration from multiple sources in a specific order. The **"last one wins"** rule applies: if the same key exists in multiple sources, the value from the source added last will be used.

Common providers (in default order):
1.  **appsettings.json**: Default configuration file.
2.  **appsettings.{Environment}.json**: Environment-specific settings (e.g., `appsettings.Development.json`).
3.  **User Secrets**: Stored outside the project tree (Development only).
4.  **Environment Variables**: Best for production secrets.
5.  **Command-line arguments**.

---

## 3. Accessing Configuration
You can access configuration data by injecting `IConfiguration` into your controllers or services.

```json
// appsettings.json
{
  "MySettings": {
    "ApiKey": "12345",
    "MaxRetry": 3
  }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }

    public IActionResult Index()
    {
        // Use a colon (:) to access nested settings
        string apiKey = _config["MySettings:ApiKey"];
        return View();
    }
}
```

---

## 4. The Options Pattern (Strongly Typed Settings)
The Options pattern uses classes to provide strongly typed access to groups of related settings. This is the preferred way to access configuration as it avoids "magic strings" and supports validation.

### 1. Define a Settings Class
The class must be non-abstract and have a public parameterless constructor.
```csharp
public class MySettings
{
    public string ApiKey { get; set; }
    public int MaxRetry { get; set; }
}
```

### 2. Register the Section in Program.cs
```csharp
builder.Services.Configure<MySettings>(
    builder.Configuration.GetSection("MySettings"));
```

### 3. Inject IOptions<T>
```csharp
public class MyService
{
    private readonly MySettings _settings;

    public MyService(IOptions<MySettings> options)
    {
        _settings = options.Value;
    }

    public void DoWork()
    {
        var key = _settings.ApiKey;
    }
}
```

---

## 5. Storing Secrets Safely

### User Secrets (Development)
User secrets are stored in a JSON file in the user profile folder, preventing them from being accidentally committed to source control.
- Right-click project > **Manage User Secrets**.
- This adds a `<UserSecretsId>` to your `.csproj` file.

### Environment Variables (Production)
In production environments (like Azure or Docker), use environment variables to store secrets. ASP.NET Core automatically maps these to configuration keys. Note that on some systems, you should use a double underscore (`__`) instead of a colon as a separator (e.g., `MySettings__ApiKey`).
