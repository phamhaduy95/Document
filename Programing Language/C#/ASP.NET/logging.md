# Logging in ASP.NET Core

Logging is an essential part of application development, providing diagnostic information that helps identify and resolve issues. Unlike simple exceptions, logs can record a chain of events leading up to an error.

## 1. Core Concepts
To work effectively with the ASP.NET Core logging framework, you should understand these three concepts:

- **LogLevel**: An enum defining the importance of a log message.
- **ILogger**: The main interface used for logging. It is typically injected into services via Dependency Injection (DI).
- **Logging Provider**: Responsible for creating `ILogger` instances and determining where the logs are sent (e.g., Console, Debug, Azure, etc.).

---

## 2. Using ILogger in a Controller
You can inject `ILogger<T>` into your classes to enable logging.

```csharp
public class SampleController : ControllerBase
{
    private readonly ILogger<SampleController> _logger;

    public SampleController(ILogger<SampleController> logger)
    {
        _logger = logger;
    }

    public IActionResult Index()
    {
        _logger.LogInformation("Entering Index action method at {Time}", DateTime.UtcNow);
        return Ok("Success");
    }
}
```

---

## 3. Log Levels
There are six log levels, ordered here from most to least serious:

| Level | Description |
| :--- | :--- |
| **Critical** | Disastrous failures (e.g., out of memory, server failure). |
| **Error** | Unhandled exceptions in the current operation; app can still function for others. |
| **Warning** | Unexpected conditions that can be worked around (e.g., entity not found). |
| **Information** | General flow of the application (e.g., user login). |
| **Debug** | Detailed info useful during development. |
| **Trace** | Highly detailed info, potentially containing sensitive data. |

---

## 4. Logging Providers
Providers control the destination of your log messages. Common built-in providers include:

- **Console**: Writes to the standard output.
- **Debug**: Writes to the IDE's debug window.
- **EventLog**: Writes to the Windows Event Log (Windows only).
- **EventSource**: Uses Event Tracing for Windows (ETW) or LTTng on Linux.

---

## 5. Components of a Log Message
Each log record typically includes:
- **Log Level**: Importance (e.g., `Information`).
- **Event Category**: Typically the full name of the class creating the log.
- **Message**: The text, often containing placeholders (e.g., `Processing item {ItemId}`).
- **Exception**: Optional exception object passed along with the message.
- **EventId**: Optional integer to group similar types of events.
