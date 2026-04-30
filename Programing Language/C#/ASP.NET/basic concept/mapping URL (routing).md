ROUTING

1.  Introduction to routing in ASP.NET core

Routing in ASP.NET Core is the process of mapping an incoming HTTP
request to a specific handler. In legacy MVC framework, the handler is
Action from the Controller while it is the Page handler in Razor
framework.

The Routing task of ASP.NET core is managed by the combination of two
middleware components:

- *Endpoint Middleware*: You use this middleware to register the HTTP
  request handlers in your application. The handlers mentioned here can
  be the action method within the Controller class or Page handlers from
  Razor Page.

- *Routing Middleware*: This [middleware]{.mark} chooses which of the
  endpoints registered by the EndpointMiddleware should execute for a
  given request at runtime.

> ![](media/image1.png){width="6.5in" height="3.8819444444444446in"}

*figure 1: the diagram illustrates how the routing mechanism is executed
ASP.NET core.*

**Note**: If the request URL does not match a route template, no
endpoint is selected or executed. The whole middleware pipeline is still
executed, but typically a 404 response is returned when the request
reaches the dummy 404 middleware.

2.  Defining routing

**2.1 route template**

One common problem arising when mapping URL to different endpoint is
that you often need to create many URL strings whose value varies
little. For instance, a typical Ecommerce size usually assigns a unique
URL for getting page for each product within its inventory. And these
URLs differ only by the segment Id at the end of URL sting:

shop/product/0dda13 -\> page for apple.

shop/product/0dda45 -\> page for orange.

To address this issue, ASP.NET support Routing templates which define
the pattern for URLs in your application. One route template can be used
to represent multiple URL strings which follow the pattern from the
route template.

Let explore route template syntax to produce the correct URL for your
app

The route template comprises of several segments which are separated by
the dash (/). For each segment we can define it as:

- *literal value*: /literal_value.

- *variable parameter*: /{variable}.

- *optional parameter*: /{optional?}.

- *variable parameter with default value*: /{variable=default_value}

- *segment with value constraints*: / {id: int}

![](media/image2.png){width="2.8126727909011375in"
height="1.3231660104986878in"}

For example, the route template above considers these URL as valid.

  -----------------------------------------------------------------------
  URL                           Route value
  ----------------------------- -----------------------------------------
  product/fruit/orange          category = fruit, name = orange

  product/car                   category = car, name = all

  product/meat/pork/0125        category = meat, name = pork, id=0125
  -----------------------------------------------------------------------

you can add constraints to the URL string through route template as
well. To define the constraints in your route template, use colon (:)
within a variable segment. For example: /{age: min(18)}.

**Warning**: Don't use route constraints for data validation. This duty
should be done in model binding phase instead.

The complete list of route constraints type can be found in in
Microsoft's "Routing in ASP.NET Core" documentation.

Some rules when designing route template:

- The optional segment must be put at the end of the URL string.

- Some words can't be used as literal segment: area, action, controller,
  handler, and page. These still can be used as variable or optional
  one. For example, controller/page is illegal while
  {controller}/{page=index} is accepted.

- Routing is not case sensitive. For example: "Shop/inventory" is the
  same as "shop/Inventory".

- Those words controller, page, action when are used as variable, they
  will be automatically mapped to the name of the Controller, Page,
  Action declared in ASP.NET core application respectively.

**2.2 convention-base routing vs attribute routing**

ASP.NET supports two different approaches for define URL-endpoint
mapping:

- ***Convention-base routing*:** can be defined globally in program.cs.
  It is typically used with controllers and views in MVC framework.

> in ASP.NET core MapControllerRoute is standard method for creating a
> single route. Each Single route defined by this method requires name,
> and the route template and sometimes a default value. Beside the
> routing definition task, the MapControllerRoute automatically
> registers the endpoint for you as well so no need to call UseEndpoint.
>
> app.MapControllerRoute (
>
> name: \"default\",
>
> pattern: \"{controller=Home}/{action=Index}/{id?}\");
>
> Multiple conventional routes are defined by assigning each route
> template to different MapControllerRoute. For example
>
> app.MapControllerRoute(name: \"blog\",
>
> pattern: \"blog/{\*article}\",
>
> defaults: new {controller = \"Blog\", action = \"Article\"});
>
> app.MapControllerRoute(name: \"default\",
>
> pattern: \"{controller=Home}/{action=Index}/{id?}\");

- **Attribute routing:** is often used for RESTful APIs application
  where the HTTP request contains HTTP verbs such as POST, PUT, GET,
  DELETE, ... It uses a set of attributes to map actions directly to
  route templates.

> To make the app use attribute routing, call MapController in your
> program.cs.
>
> var builder = WebApplication.CreateBuilder(args);
>
> builder.Services.AddControllers();
>
> var app = builder.Build();
>
> app.UseHttpsRedirection();
>
> app.UseAuthorization();
>
> app.MapControllers();
>
> app.Run();
>
> To map a Route template to one Action within API controller, add
> Route("route_template") attribute to that Action. For example:
>
> public class HomeController : Controller{
>
> \[Route(\"\")\]
>
> \[Route(\"Home\")\]
>
> \[Route(\"Home/Index\")\]
>
> \[Route(\"Home/Index/{id?}\")\]
>
> public IActionResult Index(int? id) {
>
> return ControllerContext.MyDisplayRouteInfo(id);
>
> }
>
> }
