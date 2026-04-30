proxy server provides these viable benefits

- Security---Reverse proxies are specifically designed to be exposed to
  malicious internet traffic, so they're typically well-tested and
  battle-hardened.

- Performance---You can configure reverse proxies to provide performance
  improvements by aggressively caching responses to requests.

- Process management---An unfortunate reality is that apps sometimes
  crash. Some reverse proxies can act as monitors/schedulers to ensure
  that if an app crashes, the proxy can automatically restart it.

- Support for multiple apps---It's common to have multiple apps running
  on a single server. Using a reverse proxy makes it easier to support
  this scenario by using the host name of a request to decide which app
  should receive the request.

Hosting with ISS server

The first step in preparing IIS to host ASP.NET Core applications is to
install the\
ASP.NET Core Windows Hosting Bundle.3 This includes several components
needed\
to run .NET apps:\
ϒ *The .NET Runtime* ---Runs your .NET 5.0 application\
ϒ *The ASP.NET Core Runtime*---Required to run ASP.NET Core apps\
ϒ *The IIS AspNetCore Module*---Provides the link between IIS and your
app, so thatIIS can act as a reverse proxy

Once you've installed the bundle, you need to configure an *application
pool* in IIS for\
your ASP.NET Core apps.

An *application pool* in IIS represents an application process. You\
can run each app in IIS in a separate application pool to keep them
isolated\
from one another

![](media/image1.png){width="6.5in" height="1.9569444444444444in"}

![](media/image2.png){width="6.5in" height="3.0590277777777777in"}

You need to carry out one more critical setup step before you can
publish and run\
your app: you must grant permissions for the NetCore app pool to access
the path\
where you'll publish your app. To do this, right-click the folder that
will host your app\
in Windows File Explorer and choose Properties. In the Properties dialog
box, choose\
Security \> Edit \> Add. Enter IIS AppPool\\NetCore in the text box

IIS integration is added by default when you use the
IHostBuilder.ConfigureWebHostDefaults() helper method used in the
default templates. If you're customizing your own HostBuilder, you need
to ensure you add IIS integration with the\
UseIIS() or UseIISIntegration() extension method.

Hosting with NginX
