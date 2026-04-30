# Data Protection in ASP.NET Core

The ASP.NET Core Data Protection stack provides a simple, user-friendly interface for encrypting and decrypting data. It is primarily used for short-term protection of sensitive strings (like authentication cookies).

---

## 1. How It Works
Protecting data involves three main steps:

1.  **Create a Data Protector** using a data protection provider.
2.  **Protect**: Encrypt the plain text.
3.  **Unprotect**: Decrypt the protected payload back into plain text.

### Example Usage
```csharp
public class MyService
{
    private readonly IDataProtector _protector;

    // The provider is injected via Dependency Injection
    public MyService(IDataProtectionProvider provider)
    {
        // "Purpose strings" provide cryptographic isolation
        _protector = provider.CreateProtector("Contoso.MyClass.v1");
    }

    public void SecureData(string sensitiveInfo)
    {
        // Encrypt the payload
        string protectedData = _protector.Protect(sensitiveInfo);
        Console.WriteLine($"Protected: {protectedData}");

        // Decrypt the payload
        string originalData = _protector.Unprotect(protectedData);
        Console.WriteLine($"Original: {originalData}");
    }
}
```

---

## 2. Purpose Strings
When you create a protector, you must provide one or more **Purpose Strings**. These strings provide isolation between cryptographic consumers. Data protected with a purpose of "green" cannot be unprotected by a protector with a purpose of "purple", even if they use the same underlying root keys.

---

## 3. Key Persistence
By default, keys are stored in the user profile folder. In production or containerized environments, you should configure a persistent storage location so that sessions remain valid across deployments or restarts.

### Persisting to a Database (EF Core)
```csharp
builder.Services.AddDataProtection()
    .PersistKeysToDbContext<AppDbContext>();
```

### Key Lifetime
You can configure how long keys remain valid before a new one is generated.
```csharp
builder.Services.AddDataProtection()
    .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

---

## 4. Hosting in Containers
When running in Docker, keys must be stored in a location that persists beyond the container's lifetime:
- **Docker Volumes**: A shared or host-mounted volume.
- **External Providers**: Azure Blob Storage, Redis, or HashiCorp Vault.

> [!WARNING]
> If keys are lost (e.g., when a container is deleted without a persistent volume), any data protected with those keys—including active user sessions and authentication cookies—will become unreadable and invalid.
