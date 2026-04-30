Adding Authentication to your ASP.NET core app

There are several concepts you need to understand to effectively
configure the authentication middleware:

- *the authentication scheme*: is a name that corresponds to a specific
  authentication handler alongside with its option for configuration.

- *An authentication handler:* is the main implementation for
  authenticating users. The authentication handler returns an
  AuthenticateResult indicating whether the authentication was
  successful or not.

To register the authentication scheme to the middleware, call
AddAuthentication(string scheme) which makes the input scheme as the
default implementation and then call any scheme-specific extension
method such as AddJwtBearer or Addcookie to provide more detail and
option for configuration.

For example: we register two authentication schemes, one using JWT
bearer and one choosing cookie as main approach.

builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)

.AddJwtBearer(JwtBearerDefaults.AuthenticationScheme, options =\>
builder.Configuration.Bind(\"JwtSettings\", options))

.AddCookie(CookieAuthenticationDefaults.AuthenticationScheme,

options =\> builder.Configuration.Bind(\"CookieSettings\", options));

After providing some setting, we can add the middleware by calling
app.UseAuthentication()

Configure options for authentication scheme

Instead of providing the string value for default authentication
scheme's name, we can use lambda action as input for AddAuthentication
method to give further configuration.

any authentication register method uses the AuthenticationSchemeOptions
to provide the configuration for the scheme.

you can forward the one responsibility to other scheme using method from
AuthenticationSchemeOptions.

- ForwardAuthenticate: If set, this specifies the target scheme that
  this scheme should forward AuthenticateAsync calls to.

- ForwardChallenge: If set, this specifies the target scheme that this
  scheme should forward ChallengeAsync calls to.

- ForwardForbid: If set, this specifies the target scheme that this
  scheme should forward ForbidAsync calls to.

- ForwardSignIn: If set, this specifies the target scheme that this
  scheme should forward SignInAsync calls to.

For example: we want to build policy scheme which might use Google
authentication for challenges, and cookie authentication for everything
else.

services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)

.AddCookie(options =\> options.ForwardChallenge = \"Google\")

.AddGoogle(options =\> { });
