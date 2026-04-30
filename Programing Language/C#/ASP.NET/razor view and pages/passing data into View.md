PASSING DATA INTO VIEW

There are three ways for injecting data into Razor View:

- using strongly-typed data

- ViewData

- ViewBag

1.  using strongly-typed data

The returned View() inside action method can accept an object which hold
data for passing into Razor view. The object is often called as the view
model. The major advantage of this approach is its strong type
enforcement which ensure type safety and the intelliSence support which
helps fast development.

To pass data using view model, follow these steps below:

step 1: define the viewModel class. The view model class should be
public and consists of public gettable data members while not having any
method.

namespace WebApplication1.ViewModels

public class AddressViewModel

{

public string Name { get; set; }

public string Street { get; set; }

public string City { get; set; }

public string State { get; set; }

public string PostalCode { get; set; }

}

step 2: initiate the object from view model and pass it into View( ) as
a parameter.

public IActionResult Contact()

{

var viewModel = new Address()

{

Name = \"Microsoft\",

Street = \"One Microsoft Way\",

City = \"Redmond\",

State = \"WA\",

PostalCode = \"98052-6399\"

};

return View(viewModel);

}

step 3: To use it on Razor View file (.cshtml), specify a model using
\@model directive then you can acquire data from the model with \@Model:

\@model WebApplication1.ViewModels.AddressViewModel

\<h2\>Contact\</h2\>

\<address\>

\@Model.Street\<br\>

\@Model.City, \@Model.State \@Model.PostalCode\<br\>

\<abbr title=\"Phone\"\>P:\</abbr\> 425.555.0100

\</address\>

2.  ViewData

ViewData is a
[ViewDataDictionary](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)
object accessed through string keys. To add data into ViewData object,
use the syntax:

ViewData\[\"key_string\"\] = value;

The value assign to ViewData can be string, primitive type or an object.

public IActionResult SomeAction()

{

ViewData\[\"Greeting\"\] = \"Hello\";

ViewData\[\"Address\"\] = new Address()

{

Name = \"Steve\",

Street = \"123 Main St\",

City = \"Hudson\",

State = \"OH\",

PostalCode = \"44236\"

};

return View();

}

in Razor view, if the value inside ViewData is string and numeric value,
you can inject the value directly to Razor view. For other complex type
such as object, you need to explicitly cast the ViewData value to
correct type.

\@{

// Since Address isn\'t a string, it requires a cast.

var address = ViewData\[\"Address\"\] as Address;

}

// since ViewData\[\"Greeting\"\] is string so its value is
automatically inserted.

\@ViewData\[\"Greeting\"\] World!

\<address\>

\@address.Name\<br\>

\@address.Street\<br\>

\@address.City, \@address.State \@address.PostalCode

\</address\>

3.  ViewBag

ViewBag is a C# object that provides dynamic access to the objects
stored in ViewData. ViewBag can be more convenient to work with, since
it doesn\'t require casting and querying data using string. With
ViewBag, you can dynamically access the data within ViewBag just like
working with regular object, however, Visual studio does not provide
intellisense for dynamic objects including ViewBag like VS does for
strongly-typed data.

C#

public IActionResult SomeAction()

{

ViewBag.Greeting = \"Hello\";

ViewBag.Address = new Address()

{

Name = \"Steve\",

Street = \"123 Main St\",

City = \"Hudson\",

State = \"OH\",

PostalCode = \"44236\"

};

return View();

}

**.cshtml**

\@ViewBag.Greeting World!

\<address\>

\@ViewBag.Address.Name\<br\>

\@ViewBag.Address.Street\<br\>

\@ViewBag.Address.City, \@ViewBag.Address.State
\@ViewBag.Address.PostalCode

\</address\>

4.  type casting ViewBag and ViewData.

The ViewBag and ViewData use boxing mechanism to wrap around an
arbitrary object. You can unbox the data held by ViewBag and ViewData to
transform it back to it true original type. Doing so allow the
IntelliSence feature support while working with data.

Note: Please ensure the correct type for type casting the when unboxing
the data inside ViewData or ViewBag;

@\*import the correct type of View Model for the data \*@

\@using StudentViewModel;

\@{

var student = (StudentViewModel) ViewBag.Student;

}

\<div\>

\<p\> Student name: \@student.Name \</p\>

\<p\> Student age: \@student.Age \</p\>

\</div\>
