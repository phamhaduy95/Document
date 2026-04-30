ASP.NET Middleware

1.  What is Middleware

The main duty of a typical ASP.NET core program is to receive incoming
HTTP requests and send back suitable HTTP responses. To efficiently
accomplish this task, ASP.NET core framework implements middleware
pattern which contains the chain of middleware components, each of which
is delegated with different and unique responsibility. Middleware is
implemented as C# class which is capable of:

- Handling an incoming HTTP request by generating an HTTP response.

- Processing an incoming HTTP request, modify it, and pass it on to
  another piece of middleware.

- Processing an outgoing HTTP response, modify it, and pass it on to
  either another piece of middleware or the ASP.NET Core web serve.

The middleware components are often chained with each other to create a
pipeline. The diagram below illustrates a simple sample of middleware
pipeline.

![Request processing pattern showing a request arriving, processing
through three middlewares, and the response leaving the app. Each
middleware runs its logic and hands off the request to the next
middleware at the next() statement. After the third middleware processes
the request, the request passes back through the prior two middlewares
in reverse order for additional processing after their next() statements
before leaving the app as a response to the
client.](media/image1.png){width="6.245138888888889in" height="4.0in"}

Through the diagram showed above, you can acquire some useful insights
about middleware pattern used in ASP.NET core:

- The middleware pipeline is bidirectional as the middleware components
  does not only process any incoming HTTP request but also any HTTP
  responses propagated from the lower middleware in the pipeline.

- The final piece in the middleware pipeline is the endpoint Middleware
  which play the most important and irreplaceable role in ASP.NET core
  since it handles most of HTTP requests and generates a majority of
  HTML pages and API responses.

- Each middleware component, which is contained within the pipeline, is
  delegated with a piece of cross-cutting concerns in your application.
  These includes things like logging, security, exception and error
  handling, URL routing, static resources transmitting, and so on.

One powerful feature of ASP.NET middleware is the ability to perform
*short-circuit* which prevent passing HTTP request further in the
pipeline but instead generate the HTTP response directly.

2.  Adding middleware pipeline to your project.

Integrating middleware pipeline into your project is essential duty to
build successful web application as it helps separate many aspects the
HTTP processing tasks and delegate each of them into smaller module.
ASP.NET offers a variety of useful built-in middleware to work with. And
the program.cs is the place You can add these middleware components.

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllersWithViews();

builder.Services.Configure\<MySetting\>(builder.Configuration.GetSection(\"MySettings\"));

var app = builder.Build();

// Configure middleware pipeline.

if (!app.Environment.IsDevelopment())

{

app.UseExceptionHandler(\"/Home/Error\");

app.UseHsts();

}

app.UseHttpsRedirection();

app.UseStaticFiles();

app.UseRouting(); // add Routing Middleware

app.UseAuthorization(); // add Authorization Middleware

// register an endpoint middleware

app.MapControllerRoute(

name: \"default\",

pattern: \"{controller=Home}/{action=Index}/{id?}\");

app.Run();

Among these middleware components, Routing Middle and Endpoint Middle
are the most crucial middleware pieces as Routing Middle serves as
request mapper which redirect any HTTP request to the correct Action and
Page Handler in the controller while Endpoint Middleware is where the
Controller or the Razor Page
