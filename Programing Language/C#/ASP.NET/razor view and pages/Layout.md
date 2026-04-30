A layout in Razor is a template that includes common code. It can't be
rendered directly, but it can be rendered in conjunction with normal
Razor views.

Using layouts for shared markup

Layout files are, for the most part, normal Razor templates that contain
markup common to more than one page. An ASP.NET Core app can have
multiple layouts, and layouts can reference other layouts.

A common convention is to prefix your layout files with an underscore
(\_) to distinguish them from standard Razor templates in your Pages
folder.

A layout file looks similar to a normal Razor template, with one
exception: every layout must call the \@RenderBody() function. This
tells the templating engine where to insert the content from the child
views.

\<!DOCTYPE html\>\
\<html\>\
\<head\>\
\<meta charset=\"utf-8\" /\>\
\<title\>@ViewData\[\"Title\"\]\</title\>\
\<link rel=\"stylesheet\" href=\"\~/css/site.css\" /\>\
\</head\>\
\<body\>\
\@RenderBody()\
\</body\>\
\</html\>

Overriding parent layouts using sections

Sections provide a way of organizing where view elements should be
placed within a layout. They're defined in the view using an \@section
definition.

\@{\
Layout = \"\_TwoColumn\";\
}\
\@section Sidebar {\
\<p\>This is the sidebar content\</p\>\
}\
\<p\>This is the main content \</p\>

\@{\
Layout = \"\_Layout\";\
}\
\<div class=\"main-content\"\>\
\@RenderBody()\
\</div\>\
\<div class=\"side-bar\"\>\
\@RenderSection(\"Sidebar\", required: true)\
\</div\>\
\@RenderSection(\"Scripts\", required: false)

They're perfect for avoiding duplication of content that you'd need to
write for every view.

They provide a\
means of breaking up a larger view into smaller, reusable chunks

Partial views are rendered using the \<partial /\>

Partial views are a bit like Razor Pages without the PageModel and
handlers. Partial views are purely about rendering small sections of
HTML, rather than handling requests, model binding, and validation, and
calling the application model.

Partial views can bind to data in the Model property, like a normal
Razor Page uses a PageModel.

partial view

\<h2\>@Model.Title\</h2\>\
\<ul\>\
\@foreach (var task in Model.Tasks)\
{\
\<li\>@task\</li\>\
}\
\</ul\>

render partial view

\@page

\@model RecentToDoListModel

\@foreach(var todo in Model.RecentItems)

{

\<partial name=\"\_ToDo\" model=\"todo\" /\>

}

NOTE Like layouts, partial views are typically named with a leading
underscore.
