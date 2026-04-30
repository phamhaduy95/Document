*19.1.1 Creating simple endpoints with the Run extensi*

You can use the Run extension method to build a simple middleware
component\
that always generates a response. This extension takes a single lambda
function that\
runs whenever a request reaches the component. The Run extension always
generates. a response, so no middleware placed after it will ever
execute. For that reason, you\
should always place the Run middleware last in a middleware pipeline.

TIP Remember, middleware runs in the order you add them to the
pipeline.\
If a middleware component handles a request and generates a response,
later\
middleware will never see the request

The Run extension method provides access to the request in the form of
the HttpContext object you saw in chapter 3. This contains all the
details of the request in the\
Request property, such as the URL path, the headers, and the body of the
request. It\
also contains a Response property you can use to return a response

![](media/image1.png){width="6.5in" height="2.415277777777778in"}

*3 Adding to the pipeline with the Use extension*

With the Use extension, you have control over when, and if, you call the
rest of the middleware pipeline. But it's important to note that you
generally shouldn't modify the Response object after calling next().
Calling next() runs the rest of the middleware pipeline, so a subsequent
middleware may start streaming the response to the browser. If you try
to modify the response *after* executing the pipeline, you may end up\
corrupting the response or sending invalid data.

Don't *modify* the Response object after calling next(). Also, don't
call\
next() if you've written to the Response.Body; writing to this Stream
can trigger\
Kestrel to start streaming the response to the browser, and you could
cause\
invalid data to be sent. You can generally *read* from the Response
object safely;\
for example, to inspect the final StatusCode or ContentType of the
response.

*Building a custom middleware component.*

In some cases you may need to use DI to inject services and use them to\
handle a request. You can inject singleton services into the constructor
of your middleware component, or you can inject services with any
lifetime into the Invoke\
method of your middleware,

public class ExampleMiddleware

{

private readonly RequestDelegate \_next;

private readonly ServiceA \_a;

public HeadersMiddleware(RequestDelegate next, ServiceA a)

{

\_next = next;

\_a = a;

}

public async Task Invoke(

HttpContext context, ServiceB b, ServiceC c)

{

// use services a, b, and c

// and/or call \_next.Invoke(context);

}

}

ASP.NET Core creates the middleware only once for the lifetime of your
app, so any dependencies injected in the constructor must be singletons.
If you need to use scoped or transient dependencies, inject them into
the Invoke method.
