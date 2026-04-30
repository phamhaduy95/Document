# Background Services in C#

In most applications, it's common to create tasks that run in the background rather than in response to a direct request. Examples include processing email queues, handling message bus events, or running batch processes. Moving this work to background tasks keeps your user interface responsive.

## IHostedService Interface
In ASP.NET Core, you can use the `IHostedService` interface to run background tasks. Classes implementing this interface start when the application starts and stop when it shuts down.

```csharp
public interface IHostedService 
{
    Task StartAsync(CancellationToken cancellationToken);
    Task StopAsync(CancellationToken cancellationToken);
}
```

> [!WARNING]
> Calling `await` in `IHostedService.StartAsync()` will block your application from starting until the method completes.

## BackgroundService Base Class
To simplify background tasks, ASP.NET Core provides the `BackgroundService` abstract base class. It implements `IHostedService` and is optimized for long-running tasks. You only need to override the `ExecuteAsync` method.

```csharp
public class MyBackgroundService : BackgroundService
{
    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            // Do background work here
            await Task.Delay(1000, stoppingToken);
        }
    }
}
```

### Registration
Register your background service in `Program.cs` (or `Startup.cs`):

```csharp
services.AddHostedService<MyBackgroundService>();
```

## Using Scoped Services in Background Tasks
Background services are registered as **singletons**. This means you cannot directly inject **scoped** services (like an Entity Framework `DbContext`) into their constructors.

To use a scoped service, you must manually create a scope using `IServiceProvider`.

> [!WARNING]
> Always dispose of the `IServiceScope` (typically with a `using` statement) to prevent memory leaks.

### Example: Using Scoped Services
```csharp
public class ExchangeRatesHostedService : BackgroundService
{
    private readonly IServiceProvider _provider;

    public ExchangeRatesHostedService(IServiceProvider provider)
    {
        _provider = provider;
    }

    protected override async Task ExecuteAsync(CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            using (IServiceScope scope = _provider.CreateScope())
            {
                var client = scope.ServiceProvider.GetRequiredService<ExchangeRatesClient>();
                var context = scope.ServiceProvider.GetRequiredService<AppDbContext>();

                var rates = await client.GetLatestRatesAsync();
                context.Add(rates);
                await context.SaveChangesAsync(stoppingToken);
            }

            // Wait before the next execution
            await Task.Delay(TimeSpan.FromMinutes(5), stoppingToken);
        }
    }
}
```
