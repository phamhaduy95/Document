WEB APIs

1.  Introduction

Web APIs shares the same structure and architect with MVC, along with
many important concepts such as routing, model binding and validation.
The only differences between Web APIs and traditional MVC is the view
part, instead of returning HTML page, the Web APIs return data as JSON
or XML. A Web APIs exposes several URLs that can be used to access or
modify the data in server using HTTP.

2.  Creating Web APIs app

Visual Studio provides a standard template for building a typical Web
APIs application.

![](media/image1.png){width="6.5in" height="4.177083333333333in"}

Then IDE will generate the default project which contain the program.cs
file below:

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllers();

// Learn more about configuring Swagger/ OpenAPI at
https://aka.ms/aspnetcore/swashbuckle

builder.Services.AddEndpointsApiExplorer();

builder.Services.AddSwaggerGen();

var app = builder.Build();

// Configure the HTTP request pipeline.

if (app.Environment.IsDevelopment())

{

// enable API testing tool SwaggerUI

app.UseSwagger();

app.UseSwaggerUI();

}

app.UseHttpsRedirection();

app.UseAuthorization();

// register Web API endpoint

app.MapControllers();

app.Run();

The SwaggerUI is a powerful built-in tool for testing APIs. It has a
various options and configuration settings for developer to facilitate
the API testing process. The more in-depth about the SwaggerUI will be
mentioned in another document. Alternatively, you can opt to other API
testing tool such as Postman if you find more familiar with these tools.

3.  **API controller**

Since Web APIs design is based on MVC pattern, the main unit for
processing HTTP request is the Controller. Just like regular Controller,
it requires some configuration before running, one of which is to define
routing.

**3.1 Configure Routing Api controller**

Web APIs Controller use the attribute routing to configure route for
each action method inside the Controller. To add route template for any
action method in the Controller, add \[Route("")\] on top of the action
method.

\[Route(\"api/\[controller\]\")\] // specific the root URL "api/Student"
for all action method within controller.

\[ApiController\]

public class StudentController : ControllerBase{

\[HttpGet\] // specify the HTTP verb

> \[Route(\"{Id}\")\] // extending the root URL with more segments. For
> example: api/Student/stu_001

public IActionResult getStudent(string Id){

return Ok(\$\"Student {Id} data\");

}

}

> *Note: Don't forget to subclass your Controller with ControllerBase
> which is a base class for MVC controller with no Razor View supports.*

A single action method can still have multiple route templates and hence
can correspond to multiple URLs. For example:

\[Route(\"api/car\")\]

\[ApiController\]

public class CarController: ControllerBase

{

> // Start method is assigned three corresponded URLs. "api/car/start",
> "api/car/run" and "run-car" URLs are all accepted.

\[Route(\"start\")\] // combine "start" with "api/car"

\[Route(\"run\")\] // combine "run" with base URL

> \[Route(\"/run-car\")\] // the slash specify that this is new
> separated URL from the base one.

public IActionResult Start () {

return Ok(\"the car is started\");

}

}

The \[Route\] attribute support the route combination mechanism which
facilitates construction process of URL. Like the example above, use
\[Route("start")\] help combine the "start" with base URL "api/car" of
CarController. The slash when added at the start will help create an
absolute route template.

you can also inject the name of Controller and Action using token
replacement. For example:

\[Route(\"api/\[controller\]\")\]

\[ApiController\]

public class StudentController : ControllerBase{

\[HttpGet\]

> \[Route(\"\[action\]/{Id}\")\] // the template route for this action
> is \"api/Student/GetStudent/{Id}\"

public IActionResult GetStudent(string Id){

return Ok();

}

}

2.  **Handling HTTP verbs**

For API controllers, the HTTP verb takes part in the routing process
itself, so a GET request may be routed to one action, and a POST request
may be routed to a different action, even though the request used the
same URL. This pattern, where the HTTP verb is an important part of
routing, is common in HTTP API design.

ASP.NET Core provides a set of attributes that you can use to indicate
which verb\
an action should respond to. For example,

- \[HttpPost\] handles POST requests only.

- \[HttpGet\] handles GET requests only.

- \[HttpPut\] handles PUT requests only.

- \[HttpDelete\] handle DELETE requests only.

  2.  **API Controller**

The [\[ApiController\]](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute
can be applied to a controller class to enable the following
opinionated, API-specific behaviors:

- Attribute routing requirement.

- Automatic HTTP 400 responses.

- Binding source parameter inference.

- Multipart/form-data request inference.

- Problem details for error status codes.

To apply \[ApiController\] attribute, add it on top of the Controller
class.

3\. Generate HTTP response

**3.1 Web APIs Controller return types**

The Controller of Web APIs supports three options for returning data:

- the specific type

- IEnumerable\<T\> and IAsyncEnumerable\<T\>

- IActionResult or ActionResult\<T\>

the specific type can be a primitive type of a complex data type. For
example

\[HttpGet\]

\[Route(\"\[action\]/{Id}\")\]

public StudentViewModel GetStudent(string Id){

var student = new StudentViewModel(){

Id = Id,

Name = \"Hung\",

Age = 20,

ClassName = \"12A5\",

Scoreboard = new int\[\] { 2, 3, 4, 5, 2 },

};

return student;

}

IEnumerable\<T\> is used when you want to return a list of elements.

\[HttpGet\]

\[Route(\"get-list-of-student-name\")\]

public IEnumerable\<string\> getAllStudentName() {

return new\[\] {\"Hung\", \"Van\", \"Tung\", \"Cao\", \"Nhan\"};

}

Both specific type and IEnumerable\<T\> have one significant drawback
which is the lack of HTTP status code as the returned raw data will
associate with success code 200 only.

IActionResult types represent various of HTTP status codes which can be
acquired using the built-in methods from ControllerBase superclass. Some
common methods are:

- OK(): return 200 status codes

- BadRequest(): return 400 status code.

- NotFound(): return 404 status code

the action method accepts Task\<ActionResult\>

\[HttpGet\]

\[Route(\"{categoryId}\")\]

public async Task\<ActionResult\<VCategory\>\> GetById(string
categoryId)

{

var cate = await categoryService.GetById(categoryId);

return Ok(cate);

}

3.2 Using Custom Formatter

By default, API controller make its action methods to return the HTTP
response in JSON format. However, you can change the default JSON format
to other types such as YAML, XML or text.
