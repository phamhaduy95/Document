# C# and Razor Syntax in Views

Razor is a powerful markup syntax that allows you to embed server-side C# code directly into your HTML pages. It provides a seamless way to create dynamic content based on user data or application state.

---

## 1. Basic Expressions
The **`@`** symbol is the transition character between HTML and C#.

- **Implicit Expressions**: Use `@` followed by a variable or property name to output its value.
  ```html
  <p>The current year is @DateTime.Now.Year</p>
  ```
- **Explicit Expressions**: Use **`@(...)`** when you need to perform a calculation or a method call that isn't a simple property access.
  ```html
  <p>The total price is: @(Model.Price * Model.Quantity)</p>
  ```

---

## 2. Razor Code Blocks
Use **`@{ ... }`** to write a block of C# code that doesn't output HTML immediately. This is commonly used for defining variables or setting page metadata.

```cshtml
@{
    var userStatus = Model.IsActive ? "Online" : "Offline";
    Layout = "_MainLayout";
    ViewData["Title"] = "User Profile";
}

<p>Status: @userStatus</p>
```

---

## 3. Control Structures
Razor supports all standard C# control flow statements, allowing you to build complex logic directly in the view.

### Conditionals (`@if`)
```cshtml
@if (Model.StockCount > 0)
{
    <span class="text-success">In Stock</span>
}
else
{
    <span class="text-danger">Out of Stock</span>
}
```

### Loops (`@foreach`, `@for`)
```cshtml
<ul>
    @foreach (var hobby in Model.Hobbies)
    {
        <li>@hobby</li>
    }
</ul>
```

---

## 4. Directives
Directives appear at the top of the file and provide instructions to the Razor engine.

| Directive | Purpose |
| :--- | :--- |
| **`@model`** | Specifies the type of the data object passed to the view. |
| **`@using`** | Imports a namespace so you don't have to type full class names. |
| **`@inject`** | Injects a service from the Dependency Injection container into the view. |

```cshtml
@model UserProfile
@using MyProject.Helpers
@inject IConfiguration Configuration

<h1>Welcome to @Configuration["AppName"]</h1>
```

> [!TIP]
> If you need to render plain text inside a C# code block without using HTML tags, use the **`<text>`** tag or the **`@:`** prefix to tell Razor to stop interpreting the line as code.
