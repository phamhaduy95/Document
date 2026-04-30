ASP.Net core is the open-source web development framework which is
mainly developed and maintained by Microsoft. It is complete overhaul
from the legacy .NET framework whose many great features are translated
to ASP.NET and several limitations have been fixed.

Compared to .NET framework, the ASP.NET core has some noticeable
advantages:

- support cross-platform development which enable developing and
  maintaining one single project that is deployable to the variety of OS
  such as windows, IOS or Linux-base distro.

- simplify the development process by reducing many boilerplate codes
  and redundant classes

The core structure of typical ASP.NET core program can be depicted in
the diagram below.

![](media/image1.png){width="5.864583333333333in"
height="5.020833333333333in"}

In ASP.NET Core web applications, your middleware pipeline will normally
include the EndpointMiddleware. This is typically where you write the
bulk of your application logic, calling various other classes in your
app. It also serves as the main entry point for users to interact with
your app. It typically takes one of three forms:

- *An HTML web application designed for direct use by users*---If the
  application is consumed directly by users, as in a traditional web
  application, then Razor Pages is responsible for generating the web
  pages that the user interacts with. It handles requests for URLs, it
  receives data posted using forms, and it generates the HTML that users
  use to view and navigate your app.

- *An API designed for consumption by another machine or in code*---The
  other main possibility for a web application is to serve as an API to
  backend server processes, to a mobile app, or to a client framework
  for building single-page applications (SPAs). In this case, your
  application serves data in machine-readable formats such as JSON or
  XML instead of the human-focused HTML output.

- *Both an HTML web application and an API*---It is also possible to
  have applications that serve both needs. This can let you cater to a
  wider range of clients while sharing logic in your application.
