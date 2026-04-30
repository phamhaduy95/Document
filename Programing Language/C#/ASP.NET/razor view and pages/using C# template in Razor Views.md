the razor View is represented by .cshtml file which is a special kind of
HTML that allow developer injected C# statement or external data from
Controller. This help developer dynamically modifies the content and
data inside the HTML according to the user request.

Razor view support these techniques for injecting C# statement.

- Insert variable:

> \<p\>city: \@address.city \<p\>

- Insert result from one C# statement:

> \<p\> the sum is @(num1 + num2) \<p\>

- Code block: perform complex C# statement or define some important View
  properties such as ViewData, Layout. Nothing in code block is written
  to HTML output.

> \@{
>
> ViewData\[\"Title\"\] = \"Home Page\";
>
> }

- Loop and condition statement.

> //if statement:
>
> \@if (Model.IsComplete)\
> {\
> \<strong\>Well done, you're all done!\</strong\>\
> }
>
> //for statement:
>
> \<ul\>\
> \@foreach (var task in Model.Tasks)\
> {\
> \<li\>@task\</li\>\
> }\
> \</ul\>

- import model class

\@model WebApplication1.ViewModels.AddressViewModel
