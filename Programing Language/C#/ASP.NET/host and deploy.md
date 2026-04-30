# Hosting and Deployment in ASP.NET Core

Deploying an ASP.NET Core application involves several key steps to ensure it runs reliably in a production environment.

## General Deployment Workflow
1.  **Publish**: Deploy the published application files to a folder on the hosting server.
2.  **Process Management**: Set up a process manager (like systemd or IIS) to start the app automatically and restart it if it crashes.
3.  **Reverse Proxy**: Configure a reverse proxy (like Nginx, Apache, or IIS) to forward requests from the internet to your app.

---

## Hosting on Linux with Nginx
When hosting on Linux, Nginx is commonly used as a reverse proxy in front of the **Kestrel** web server.

### 1. Why use a Reverse Proxy?
A reverse proxy provides:
- **Security**: It acts as a buffer between the internet and your application.
- **SSL Offloading**: It handles HTTPS encryption, allowing your app to run over HTTP locally.
- **Load Balancing**: It can distribute requests across multiple instances of your app.

### 2. Forwarded Headers
When using a reverse proxy, some information (like the client's original IP address or the original protocol) is lost. To fix this, Nginx adds `X-Forwarded-*` headers, and your app must be configured to read them.

```csharp
using Microsoft.AspNetCore.HttpOverrides;

var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

// Configure the app to process forwarded headers from the proxy
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
app.MapControllers();

app.Run();
```

> [!IMPORTANT]
> `UseForwardedHeaders` should generally run before other middleware like Authentication or Routing to ensure they have access to the correct client information.
