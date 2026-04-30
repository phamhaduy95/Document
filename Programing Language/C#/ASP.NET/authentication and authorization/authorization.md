**AUTHORIZATION**

**introduction**

Authentication is the process to determine which user is try to access
some restricted endpoint while authorization is the process which
examine whether the user making the request has enough privilege to
access some important resource or endpoint.

the authorization should always be executed after users finish their
identity authentication.

**Authentication in ASP.NET**

The ASP.NET core framework consists of AuthorizationMiddleware for
managing authorization aspect of your webapp. Most of work is done by
this middleware for you, however, it allows some further configuration
and extension for the authorization behavior

**Note**: when you integrate this middleware to your app, you should
place it after both the routing middleware and the authentication
middleware, but before the endpoint middleware so that it can work
properly.

app.UseHttpsRedirection();

app.UseAuthentication();

app.UseAuthorization();

app.MapControllers();

app.Run();

The authorization is about to set the protection over a specific
endpoint so that any request needs to satisfy some requirement to able
to access it. To do so, ASP.NET support data attribute \[Authorize\]
which we can attach to an action method to setup the security for it.

Note: The \[Authorize\] without any parameter will check whether the
request is made by the authenticated user or not.

\[Authorize\]

\[HttpGet\]

\[Route(\"get-name\")\]

public async Task\<IActionResult\> GetName()

{

return Ok(\"Johnathan\");

}

You can put the \[Authorize\] attribute on the top of the controller so
that all action will be secured and mark some endpoints with
\[AllowAnonymous\] attribute to lift any restriction for them.

\[Authorize\]

\[Route(\"api/\[controller\]\")\]

\[ApiController\]

public class SampleController : ControllerBase

{

public async Task\<IActionResult\> Index()

{

return Ok(\"index\");

}

\[AllowAnonymous\]

public async Task\<IActionResult\> Login()

{

return Ok();

}

}

In case any request doesn't meet the requirement, the
AuthorizationMiddleware will short-circuit the request and generate the
corresponded response which may be one of two specific types below.

- Challenge response: This response indicates the user was not
  authorized to execute the action because they weren't yet logged in.

- Forbid response : This response indicates that the user was logged in
  but didn't meet the requirements to execute the action. They didn't
  have a required claim, for example.

**Using policies for authorization**

Ensuring the user is authenticated is the simplest protection you can
apply to your app. However, you can provide more sophisticated
requirements to challenge user request by creating authorization policy.

First you add the authorization services using AddAuthorization(), and
then you can add policies by calling AddPolicy() on the
AuthorizationOptions object. From there,

You can define the policy by using some default configuration methods:

- RequireAuthenticatedUser(): The required user must be authenticated
  (similar to the default \[Authorize\] attribute).

- RequireClaim(claim, values): The user must have the specified claim.
  If provided, the claim must be one of the specified values.

- RequireUsername(username): The user must have the specified username.

- RequireAssertion(function): Executes the provided lambda function,
  which returns a bool, indicating whether the policy was satisfied.

For example:

builder.Services.AddAuthorization(builder =\>

builder.AddPolicy(\"AppPolicy\", builder=\>

{

builder.RequireClaim("Age","20");

builder.RequireAuthenticatedUser();

builder.RequireUserName(\"Daniel Hudson\");

})

) ;

Then you can apply the new policy for authorizing any endpoint.

\[Authorize(policy=\"AppPolicy\")\]

public async Task\<IActionResult\> CheckPolicyAuthorization()

{

var message = \"AppPolicy authorization is passed\";

return Ok(message);

}

**Creating custom requirement and handler**

For building more complex requirement (for example checking user age is
bigger than one specific limit), it is almost or impossible to achieve
this with regular AuthorizationPolicyBuilder described in the previous
section.

Fortunately, ASP.NET support us a standard approach to construct custom
requirements and handler for it.

The authentication requirement is the class which implement the
IAuthorizationRequirement interface.

public class IsGoldMemberRequirement : IAuthorizationRequirement

{

}

public class MinimunAgeRequirement : IAuthorizationRequirement

{

private readonly int \_minimunAge;

public MinimunAgeRequirement(int minimunAge)

{

\_minimunAge = minimunAge;

}

}

Then you can add your custom requirement to your policy.

builder.Services.AddAuthorization(builder =\>

builder.AddPolicy(\"AppPolicy\", option =\>

{

option.AddRequirements(new IsGoldMemberRequirement(),

new MinimunAgeRequirement(18));

})

) ;

To make these requirements usable for protecting endpoint, you need to
provide the handler to describe how you verify the requirement for the
request. The handler is the class which implements the
AuthorizationHandler\<TRequirement\> where TRequirement is the type for
the targeted requirement. Authorization handlers contain the logic of
how a specific IAuthorizationRequirement can be satisfied. When
executed, a handler can do one of three things:

- Mark the requirement handling as a success by calling
  context.Succeed()

- Not do anything by return Task.CompletedTask earlier.

- Explicitly fail the requirement context.Fail();

public class HasMinimunAgeHandler:

AuthorizationHandler\<HasMinimunAgeRequirement\>

{

protected override Task HandleRequirementAsync(

AuthorizationHandlerContext context,

HasMinimunAgeRequirement requirement)

{

int minimunAge = 0;

if (!context.User.HasClaim(c=\>c.Type == \"Age\"))

{

return Task.CompletedTask;

}

if (!int.TryParse(context.User.Claims.Where(c=\>c.Type ==
\"Age\").First().Value,out minimunAge))

{

return Task.CompletedTask;

}

if (minimunAge \> requirement.\_minimunAge)

{

context.Succeed(requirement);

}

else context.Fail();

return Task.CompletedTask;

}

}

One requirement class may have more than one handler. For that
requirement to be passed as succeed, we only need one requirement to be
satisfied. However, for policy, all requirement must be fulfilled.

TIP: It's possible to write very generic handlers that can be used with
multiple requirements, but I suggest sticking to handling a single
requirement only. If you need to extract some common functionality, move
it to an external service and call that from both handlers

The final step to complete your authorization implementation for the app
is to register the authorization handlers with the DI container. For
example:

builder.Services.AddAuthorization(builder =\>

builder.AddPolicy(\"AppPolicy\", option =\>

{

option.AddRequirements(new IsGoldMemberRequirement(),

new HasMinimunAgeRequirement(18));

})

) ;

**builder.Services.AddSingleton\<IAuthorizationHandler,
HasMinimunAgeHandler\>();**

**builder.Services.AddSingleton\<IAuthorizationHandler,IsGoldMemberHandler\>();**

**Authorization with a specific scheme**

You can use a specific authentication scheme as the main requirement for
authorization for an endpoint. As the request just need to pass any
authentication schemes set up in authentication middleware to be marked
as authenticated, to specify the right scheme, adding
AuthenticationSchemes = "scheme_name" to Authorize attribute.

\[Authorize(AuthenticationSchemes =
JwtBearerDefaults.AuthenticationScheme)\]

\[HttpGet\]

\[Route(\"get-name\")\]

public async Task\<IActionResult\> GetName()

{

return Ok(\"Johnathan\");

}

**Resource-based authorization**
