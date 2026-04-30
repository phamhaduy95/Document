MODEL BINDING

1.  Introduction to Model binding

The incoming HTTP request sent by client often contains some important
data which can locate in URL query string, in header as name-value pair
or JSON data within request's body. ASP.NET support binding model
mechanism that automatically extracts all related data contained inside
the request and bind data to a variable or an object and then passed it
into the Page Handler or Action method. This powerful mechanism can be
applied to Razor Page Model, Action in Controller in MVC or API. For
example:

For MVC:

\[Route(\"api/\[controller\]\")\]

\[ApiController\]

public class UserController : ControllerBase{

\[HttpGet(\"{id}\")\]

public IActionResult GetUser(string id){

var message = \$\"student {id} data\";

return Ok(message);

}

}

The parameter id of GetUser Action is a binding model that requires data
from HTTP request. When user request URL: "api/user/0115", the blinding
model process is executed to scan through the HTTP request and look for
an any id parameter within this request.

As the binding model id shares an identical name with id variable
parameter from route template, the value "0115" extracted from URL is
then populated inside binding model id and passed into GetUser Action.

Razor page Model:

2.  Binding Complex types

All previous example so far in this document use simple parameters as a
binding model. For more complicated data which consists of many pieces
of information in one single HTTP request, you need to bind this data to
.NET object. To do so, you firstly have to declare the type to represent
the binding model.

public class UserViewModel{

public Guid Id { get; set; }

public string Name { get; set; }

public int Age { get; set; }

public string TeleNumber { get; set; }

}

Then make it as the input parameter of Action method.

\[HttpPost(\"student\")\]

public IActionResult AddStudent(UserViewModel user){

Console.WriteLine(\$\"Add user {user.Id} name {user.Name}, age:
{user.Age}\");

return Ok(user);

}

Warning: The class corresponding to the binding model must be public,
have default public constructor (parameterless constructor) and contain
public and settable properties.

Apart from primitive type, you can bind to collections, lists, and
dictionaries as well.

For targets that are Collection or List of simple types, model binding
looks for matches to parameter name or property name. For example:

\[HttpPost(\"/documents\")\]

public IActionResult AddListOfDocuments(int\[\] Documents)

You could then POST data using query string to this method by providing
values in several different formats:

- Documents\[0\]=10&Documents\[1\]=20: The parameter name is used along
  with the array expression.

- \[0\]=2&\[1\]=18: The parameter name is omitted and index can be used
  to represent the collection's elements.

- Documents=20&Documents=20: Alternatively, you can omit index instead
  and keep the parameter name.

Warning: the index for numbering elements should start from 0 and have
no gap between (for instance, only element 0 and element 2 is given
value but no element 1 ).

3.  Binding source

By default, ASP.NET Core uses three different binding sources when
creating your binding models. It looks through each of these in order
and takes the first value it finds (if any) that matches the name of the
binding model:

- *Form values*---Sent in the body of an HTTP request when a form is
  sent to the server using a POST.

- *Route values*---Obtained from URL segments or through default values
  after matching a route, as you saw in chapter 5.

- *Query string values*---Passed at the end of the URL, not used during
  routing.

We can override this default behavior by specifying the exact source for
finding data:

- \[FromQuery\] - Gets values from the query string.

- \[FromRoute\] - Gets values from route data.

- \[FromForm\] - Gets values from posted form fields.

- \[FromBody\] - Gets values from the request body.

- \[FromHeader\] - Gets values from HTTP headers.

For example:

\[HttpPost(\"student\")\]

public IActionResult AddStudent(\[FromBody\] UserViewModel user)

4.  **No source for a model property**

By default, a model state error isn\'t created if no value is found for
a model property. The property is set to null or a default value:

- Nullable simple types are set to null.

- Non-nullable value types are set to default(T). For example, a
  parameter int id is set to 0.

- For complex Types, model binding creates an instance by using the
  default constructor, without setting properties.

- Arrays are set to Array.Empty\<T\>(), except that byte\[\] arrays are
  set to null.

This behavior can be modified with the model data validation which is
different topic for document.
