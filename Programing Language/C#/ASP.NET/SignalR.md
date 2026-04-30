# SignalR in ASP.NET Core

SignalR is a library for ASP.NET Core that simplifies adding real-time web functionality to apps. Real-time web functionality is the ability to have server-side code push content to connected clients instantly.

---

## 1. Hubs
SignalR uses **Hubs** to communicate between clients and servers. A Hub is a high-level pipeline that allows a client and server to call methods on each other.

### Key Characteristics:
- **Transient**: Hub instances are created for every method call. Do not store state in properties of the Hub class.
- **Asynchronous**: Always `await` asynchronous calls (like `SendAsync`) to ensure the Hub stays alive until the operation completes.

---

## 2. Configuration
Register SignalR services and map your hubs in `Program.cs`.

```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSignalR(); // Register services

var app = builder.Build();

app.MapHub<ChatHub>("/chat"); // Map the endpoint
app.Run();
```

---

## 3. Hub Context
The `Hub.Context` property provides information about the connection:

| Property | Description |
| :--- | :--- |
| **ConnectionId** | Unique ID for the connection. |
| **UserIdentifier** | Unique ID for the user (based on `NameIdentifier` claim by default). |
| **User** | The `ClaimsPrincipal` associated with the current user. |
| **Items** | A key/value collection to share data across the scope of the connection. |

---

## 4. Sending Messages from Outside a Hub
To send messages from a Controller, Middleware, or other service, inject `IHubContext<THub>`. This is useful for notifying clients about events that happen outside of a hub method.

```csharp
public class NotificationService
{
    private readonly IHubContext<ChatHub> _hubContext;

    public NotificationService(IHubContext<ChatHub> hubContext)
    {
        _hubContext = hubContext;
    }

    public async Task SendNotification(string message)
    {
        await _hubContext.Clients.All.SendAsync("ReceiveNotification", message);
    }
}
```

---

## 5. JavaScript Client Example
Using the `@microsoft/signalr` package to connect from the browser.

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chat")
    .withAutomaticReconnect()
    .build();

// Listen for messages from the server
connection.on("messageReceived", (user, message) => {
    console.log(`${user}: ${message}`);
});

// Start the connection
await connection.start();
```
