Browser security prevents a web page from making requests to a different
domain than the one that served the web page. This restriction is called
the *same-origin policy*. The same-origin policy prevents a malicious
site from reading sensitive data from another site. Sometimes, you might
want to allow other sites to make cross-origin requests to your app. For
more information, see the [[Mozilla CORS
article]{.underline}](https://developer.mozilla.org/docs/Web/HTTP/CORS).

[[Cross Origin Resource
Sharing]{.underline}](https://www.w3.org/TR/cors/) (CORS):

- Is a W3C standard that allows a server to relax the same-origin
  policy.

- Is **not** a security feature, CORS relaxes security. An API is not
  safer by allowing CORS. For more information, see [[How CORS
  works]{.underline}](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-6.0#how-cors).

- Allows a server to explicitly allow some cross-origin requests while
  rejecting others.

- Is safer and more flexible than earlier techniques, such
  as [[JSONP]{.underline}](https://docs.microsoft.com/en-us/dotnet/framework/wcf/samples/jsonp).

There are three ways to enable CORS:

- In middleware using a [[named
  policy]{.underline}](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-6.0#np) or [[default
  policy]{.underline}](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-6.0#dp).

- Using [[endpoint
  routing]{.underline}](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-6.0#ecors6).

- With
  the [[\[EnableCors\]]{.underline}](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-6.0#attr) attribute.

Using
the [[\[EnableCors\]]{.underline}](https://docs.microsoft.com/en-us/aspnet/core/security/cors?view=aspnetcore-6.0#attr) attribute
with a named policy provides the finest control in limiting endpoints
that support CORS.
