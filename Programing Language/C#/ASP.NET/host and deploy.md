# In general, to deploy an ASP.NET Core app to a hosting environment:

- Deploy the published app to a folder on the hosting server.

- Set up a process manager that starts the app when requests arrive and
  restarts the app after it crashes or the server reboots.

- For configuration of a reverse proxy, set up a reverse proxy to
  forward requests to the app.

**Host ASP.NET Core on Linux with Nginx**

This guide:

- Places an existing ASP.NET Core app behind a reverse proxy server.

- Sets up the reverse proxy server to forward requests to the Kestrel
  web server.

- Ensures the web app runs on startup as a daemon.

- Configures a process management tool to help restart the web app.

## Configure a reverse proxy server

A reverse proxy is a common setup for serving dynamic web apps. A
reverse proxy terminates the HTTP request and forwards it to the ASP.NET
Core app.

### Use a reverse proxy server

When a request arrives at the reverse proxy, it contains some
information that is lost after the request is forwarded to your app. For
example, the original request comes with the IP address of the
client/browser connecting to your app: once the request is forwarded
from the reverse proxy, the IP address is that of the reverse proxy, not
the browser. Also, if the reverse proxy is used for SSL offloading (see
chapter 18), then a request that was originally made using HTTPS may
arrive at your app as an HTTP request. The standard solution to these
issues is for the reverse proxy to add additional headers before
forwarding requests to your app. For example, the X-Forwarded-For header
identifies the original client's IP address, while the X-Forwarded-Proto
header indicates the original scheme of the request (http or https).

using Microsoft.AspNetCore.HttpOverrides;

\...

app.UseForwardedHeaders(new ForwardedHeadersOptions

{

ForwardedHeaders = ForwardedHeaders.XForwardedFor \|
ForwardedHeaders.XForwardedProto

});

app.UseAuthentication();
