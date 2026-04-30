# Hosting ASP.NET Core with Reverse Proxies (IIS & Nginx)

A reverse proxy is a server that sits in front of your web application and forwards client requests to it. Using a reverse proxy like IIS or Nginx provides several critical benefits for production environments.

---

## 1. Why Use a Reverse Proxy?
- **Security**: Reverse proxies are specifically designed to be exposed to malicious internet traffic and are typically more battle-hardened than internal web servers.
- **Performance**: You can configure proxies to handle SSL/TLS termination and aggressively cache responses to improve speed.
- **Process Management**: Proxies can act as monitors; if your application crashes, the proxy can automatically restart it.
- **Support for Multiple Apps**: A single server can host multiple apps on different domains (e.g., `api.myapp.com` and `web.myapp.com`) by using the proxy to route traffic based on the hostname.

---

## 2. Hosting with IIS (Windows)
To host on Windows, you must install the **ASP.NET Core Hosting Bundle**, which includes the .NET Runtime and the **IIS AspNetCore Module**.

### Configuration Steps:
1.  **Application Pool**: Create a new Application Pool in IIS. Set the **.NET CLR version** to **No Managed Code**, as IIS only acts as a proxy and doesn't run the .NET code directly.
2.  **Folder Permissions**: You must grant the Application Pool identity permission to access your app's files. In File Explorer, add `IIS AppPool\YourAppPoolName` to the folder's security settings.
3.  **IIS Integration**: This is enabled by default in modern ASP.NET Core templates via `WebApplication.CreateBuilder(args)`.

---

## 3. Hosting with Nginx (Linux)
On Linux, it is common to use **Nginx** as a reverse proxy for the **Kestrel** web server.

### Important: Forwarded Headers
Because the proxy sits between the user and your app, your app will see the proxy's IP address instead of the user's. To fix this, you must configure **Forwarded Headers** in `Program.cs`.

```csharp
using Microsoft.AspNetCore.HttpOverrides;

var app = builder.Build();

app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

---

## 4. Deployment Strategies
Before moving your code to a server, you must **Publish** it to create a self-contained or framework-dependent set of files.

- **Framework-Dependent**: Requires the .NET Runtime to be pre-installed on the server. The published files are small.
- **Self-Contained**: Includes the .NET Runtime within the published folder. This allows the app to run on a server that doesn't have .NET installed, but results in a much larger deployment size.

**Publish via CLI:**
```bash
dotnet publish -c Release -o ./publish
```
