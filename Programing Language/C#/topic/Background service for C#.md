In most applications it's common to create tasks that happen in the
background, rather than in response to a request. This could be a task
to process a queue of emails, handling events published to some sort of
a message bus, or running a batch process to calculate daily profits. By
moving this work to a background task, your user interface can stay
responsive.

In ASP.NET Core, you can use the IHostedService interface to run tasks
in the background. Classes that implement this interface are started
when your application starts, shortly after your application starts
handling requests, and they are stopped shortly before your application
is stopped.

You can implement a background task using the IHostedService interface.
This consists of two methods:

public interface IHostedService {

Task StartAsync(CancellationToken cancellationToken);

Task StopAsync(CancellationToken cancellationToken);

}

![](media/image1.png){width="6.5in" height="4.56875in"}

WARNING Calling await in the IHostedService.StartAsync() method will\
block your application from starting until the method completes. This
can\
be useful in some cases, but it's often not the desired behavior for
background tasks

To make it easier to create background services using best-practice
patterns, ASP.NET\
Core provides the abstract base class BackgroundService, which
implements IHostedService and is designed to be used for long-running
tasks. To create a background\
task you must override a single method of this class, ExecuteAsync()

![](media/image2.png){width="6.5in" height="4.249305555555556in"}

To register your background service with the DI container, use the
AddHostedService() extension method in the ConfigureServices() method of
Startup.cs.

services.AddHttpClient\<ExchangeRatesClient\>();\
services.AddSingleton\<ExchangeRatesCache\>();\
**services.AddHostedService\<ExchangeRatesHostedService\>();**

*Using scoped services in background tasks*

Background services that implement IHostedService are created once when
your application starts. That means they are, by necessity, singletons,
as there will only ever be a single instance of the class.\
That leads to a problem if you need to use services registered with a
scoped lifetime. Any services you inject into the constructor of your
singleton IHostedService must themselves be registered as singletons

In typical ASP.NET Core applications, the framework creates a new
container scope every time a new request is received, just before the
middleware pipeline executes. All the services that are used in that
request are fetched from the scoped container. In a background service,
however, there are no requests, so no container scopes are created. The
solution is to create your own. You can create a new container scope
anywhere you have access to an IServiceProvider by calling
IServiceProvider.CreateScope(). This creates a scoped container, which
you can use to retrieve scoped services.

WARNING Always make sure to dispose of the IServiceScope returned by
CreateScope() when you're finished with it, typically with a using
statement. This disposes of any services that were created by the scoped
container and prevents memory leaks.

public class ExchangeRatesHostedService : BackgroundService\
{\
private readonly IServiceProvider \_provider;\
public ExchangeRatesHostedService(IServiceProvider provider)\
{\
\_provider = provider;\
}\
protected override async Task ExecuteAsync(\
CancellationToken stoppingToken)\
{\
while (!stoppingToken.IsCancellationRequested)\
{\
using(IServiceScope scope = \_provider.CreateScope())\
{\
var scopedProvider = scope.ServiceProvider;

var client = scope.ServiceProvider\
.GetRequiredService\<ExchangeRatesClient\>();\
var context = scope.ServiceProvider\
.GetRequiredService\<AppDbContext\>();\
var rates= await client.GetLatestRatesAsync();\
context.Add(rates);\
await context.SaveChanges(rates);\
}\
await Task.Delay(TimeSpan.FromMinutes(5), stoppingToken);\
}\
}\
}
