# Dependency Injection in ASP.NET Core

ASP.NET Core supports the **Dependency Injection (DI)** software design pattern, which is a technique for achieving **Inversion of Control (IoC)** between classes and their dependencies.

---

## 1. What is a Dependency?
A dependency is an object that another object depends on.

### The Problem with Hard-Coding Dependencies
When you instantiate a dependency directly inside a class using `new`, you create tight coupling:
- **Difficult to test**: You cannot easily swap the dependency for a mock or stub.
- **Fragile**: Changing the implementation of the dependency requires modifying every class that uses it.
- **Complex Management**: If the dependency itself has dependencies, managing the chain of object creation becomes manual and error-prone.

```csharp
public class IndexModel
{
    // Hard-coded dependency - BAD PRACTICE
    private readonly MyService _service = new MyService();
}
```

---

## 2. Implementing Dependency Injection
To implement DI effectively, follow these three steps:

### 1. Define an Interface
Interfaces allow you to decouple the implementation from the consumer.
```csharp
public interface IMessageService
{
    void Send(string message);
}
```

### 2. Use Constructor Injection
Instead of creating the dependency, ask for it in the constructor. The DI container will provide the instance at runtime.
```csharp
public class IndexModel
{
    private readonly IMessageService _messageService;

    public IndexModel(IMessageService messageService)
    {
        _messageService = messageService;
    }
    
    public void OnGet() => _messageService.Send("Hello DI!");
}
```

### 3. Register the Service
In `Program.cs`, register the mapping between the interface and the concrete implementation.
```csharp
builder.Services.AddScoped<IMessageService, EmailService>();
```

---

## 3. Service Lifetimes
ASP.NET Core manages the lifecycle of registered services. Choosing the right lifetime is critical for performance and correctness.

| Lifetime | Description |
| :--- | :--- |
| **Transient** | Created every time they are requested. Best for lightweight, stateless services. |
| **Scoped** | Created once per client request (connection). Useful for services that share state within a single web request (like DB contexts). |
| **Singleton** | Created once and shared for the entire lifetime of the application. Best for shared state or expensive-to-create objects. |

---

## 4. Advanced Scenarios

### Registering with Lambdas (Factories)
If a service requires manual initialization or parameters that aren't in the DI container, use a factory lambda.
```csharp
builder.Services.AddSingleton<IHelper>(serviceProvider => 
    new Helper("ConfigurationValue", 123));
```

### Multiple Implementations
If you register multiple implementations for the same interface, you can inject all of them using `IEnumerable<T>`.
```csharp
// Registering multiple implementations
builder.Services.AddTransient<IMessageService, EmailService>();
builder.Services.AddTransient<IMessageService, SmsService>();

// Injecting all of them
public IndexModel(IEnumerable<IMessageService> services)
{
    foreach(var service in services) { /* ... */ }
}
```
