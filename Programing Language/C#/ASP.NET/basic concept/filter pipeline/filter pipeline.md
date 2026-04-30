The filter pipeline is a relatively simple concept, in that it provides
hooks into the normal MVC request.

With filters, you can use the hooks in the MVC request to run common
code across all, or a subset of, requests. This way you can do a wide
range of things, such as:

- Ensure a user is logged in before an action method, model binding, or
  validation runs.

- Customize the output format of particular action methods.

- Handle model validation failures before an action method is invoked.

- Catch exceptions from an action method and handle them in a special
  way.

In many ways, the filter pipeline is like a middleware pipeline, but
restricted to MVC and Razor Pages requests only. Like middleware,
filters are good for handling crosscutting concerns for your application
and are a useful tool for reducing code duplication in many cases.

![](media/image1.png){width="6.5in" height="4.493055555555555in"}

Filter and middleware comparison

- The filter pipeline is similar to the middleware pipeline in many
  ways, but there are several subtle differences that you should
  consider when deciding which approach to use. When considering the
  similarities, they have three main parallels:

- Requests pass through a middleware component on the way "in" and
  responses pass through again on the way "out." Resource, action, and
  result filters are also two-way, though authorization and exception
  filters run only once for a request, and page filters run three times.

- Middleware can short-circuit a request by returning a response,
  instead of passing it on to later middleware. Filters can also
  short-circuit the filter pipeline by returning a response.

- Middleware is often used for cross-cutting application concerns, such
  as logging, performance profiling, and exception handling. Filters
  also lend themselves to cross-cutting concerns. In contrast, there are
  three main differences between middleware and filters:

- Middleware can run for all requests; filters will only run for
  requests that reach the EndpointMiddleware and execute an API
  controller action or Razor Page.

- Filters have access to MVC constructs such as ModelState and
  IActionResults. Middleware, in general, is independent from MVC and
  Razor Pages and works at a "lower level," so it can't use these
  concepts.

- Filters can be easily applied to a subset of requests; for example,
  all actions on a single controller, or a single Razor Page. Middleware
  doesn't have this concept as a first-class idea (though you could
  achieve something similar with custom middleware components).

Where possible, consider using middleware for cross-cutting concerns.
Use filters when you need different behavior for different action
methods, or where the functionality relies on MVC concepts like
ModelState validation.

Creating a filter.

You implement a filter for a given stage by implementing one of a pair
of interfaces---one synchronous (sync), one asynchronous (async):

Authorization filters---IAuthorizationFilter or
IAsyncAuthorizationFilter

Resource filters---IResourceFilter or IAsyncResourceFilter

Action filters---IActionFilter or IAsyncActionFilter

Page filters---IPageFilter or IAsyncPageFilter

Exception filters---IExceptionFilter or IAsyncExceptionFilter

Result filters---IResultFilter or IAsyncResultFilter.

"Make example"

*Adding filters to your actions, controllers, Razor Pages,\
and globally*

filters can be scoped to specific actions or controllers, so that they
only run for certain requests. Alternatively, you can apply a filter
globally, so that it runs for every MVC action and Razor Page.

You apply filter using attribute

![](media/image2.png){width="6.5in" height="2.229861111111111in"}

Apply to controller scope

![](media/image3.png){width="6.5in" height="2.279861111111111in"}

![](media/image4.png){width="6.5in" height="2.3368055555555554in"}

*the order of filter execution*

![](media/image5.png){width="6.5in" height="3.1791666666666667in"}

By default, filters execute from the broadest scope (global) to the
narrowest (action)\
when running the \*Executing method for each stage. The filters'
\*Executed methods\
run in reverse order, from the narrowest scope (action) to the broadest
(global).

*Resource filters: Short-circuiting your action methods*

Resource filters are the first general-purpose filters in the MVC filter
pipeline.

The ASP.NET Core framework includes a few different implementations of
resource filters you can use in your apps:

- ConsumesAttribute---Can be used to restrict the allowed formats an
  action method can accept. If your action is decorated with
  \[Consumes(\"application/json\")\] but the client sends the request as
  XML, the resource filter will shortcircuit the pipeline and return a
  415 Unsupported Media Type response.

- DisableFormValueModelBindingAttribute---This filter prevents model
  binding from binding to form data in the request body. This can be
  useful if you know an action method will be handling large file
  uploads that you need to manage manually. The resource filters run
  before model binding, so you can disable the model binding for a
  single action in this way.

- The next listing shows an implementation of FeatureEnabledAttribute,
  which extracts\
  the logic from the action methods and moves it into the filter.

![](media/image6.png){width="6.5in" height="2.282638888888889in"}

This simple resource filter demonstrates a number of important concepts,
which are\
applicable to most filter types:

- The filter is an attribute as well as a filter. This lets you decorate
  your controller, action methods, and Razor Pages with it using
  \[FeatureEnabled(IsEnabled = true)\].

- The filter interface consists of two methods: \*Executing, which runs
  before model binding, and \*Executed, which runs after the result has
  been executed. You must implement both, even if you only need one for
  your use case.

- The filter execution methods provide a context object. This provides
  access to, among other things, the HttpContext for the request and
  metadata about the action method the middleware will execute.

- To short-circuit the pipeline, set the context.Result property to an
  IActionResult instance. The framework will execute this result to
  generate the response, bypassing any remaining filters in the pipeline
  and skipping the action method (or page handler) entirely. In this
  example, if the feature isn't enabled, you\
  bypass the pipeline by returning BadRequestResult, which will return a
  400 error to the client.

*Action filters: Customizing model binding and action results*

Action filters run just after model binding, before the action method
executes. Thanks to this positioning, action filters can access all the
arguments that will be used to execute the action method, which makes
them a powerful way of extracting common logic out of your actions.

NOTE Action filters don't execute for Razor Pages. Similarly, page
filters don't execute for action methods.

*Using dependency injection with filter attributes*

This was a fundamental issue with implementing them as attributes that
you decorate your actions with. C# attributes don't let you pass
dependencies into their constructors (other than constant values), and
they're created as singletons, so there's only a single instance for the
lifetime of your app.

The key is to split the filter into two. Instead of creating a class
that's both an attribute and a filter, create a filter class that
contains the functionality and an attribute that tells the framework
when and where to use the filter.
