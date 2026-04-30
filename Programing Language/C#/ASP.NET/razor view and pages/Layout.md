# Layouts and Partial Views in Razor

Layouts and Partial Views are essential for maintaining a DRY (Don't Repeat Yourself) codebase. They allow you to define common UI structures once and reuse them across your entire application.

---

## 1. Layouts
A **Layout** acts as a master template for your application. It contains the standard HTML wrapper, meta tags, and common elements like navigation bars and footers.

### The `_Layout.cshtml` File
The most important part of a layout is the **`@RenderBody()`** method. This is a placeholder where the content of individual views will be injected.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>@ViewData["Title"] - My ASP.NET App</title>
    <link rel="stylesheet" href="~/css/site.css" />
</head>
<body>
    <header>
        <nav><!-- Navigation Links --></nav>
    </header>

    <div class="container">
        @RenderBody()
    </div>

    <footer>
        <p>&copy; 2024 - My Application</p>
    </footer>
</body>
</html>
```

---

## 2. Sections
**Sections** provide a way to organize where specific view elements should be placed within a layout. They are commonly used for page-specific scripts or sidebars.

- **In Layout**: `@RenderSection("Scripts", required: false)`
- **In View**:
  ```cshtml
  @section Scripts {
      <script src="~/js/custom-logic.js"></script>
  }
  ```

---

## 3. Partial Views
**Partial Views** are specialized Razor templates used to render small, reusable chunks of HTML. Unlike full views, they do not run `_ViewStart.cshtml` and are typically intended to be rendered inside other views.

### Rendering a Partial View
The recommended way to render a partial view is using the **`<partial>`** Tag Helper.

```html
<!-- Rendering a list of items using a partial view -->
@foreach (var item in Model.Items)
{
    <partial name="_ItemSummary" model="item" />
}
```

### Example Partial (`_ItemSummary.cshtml`)
```cshtml
@model MyItem
<div class="item-card">
    <h4>@Model.Title</h4>
    <p>@Model.Description</p>
</div>
```

---

## 4. Partial Views vs. View Components
While Partial Views are great for simple HTML reuse, **View Components** are better for complex logic (e.g., a dynamic sidebar that needs to fetch data from a database). View Components have their own C# class and logic, whereas Partial Views rely entirely on the data passed to them by the parent view.

> [!TIP]
> Always prefix Layouts and Partial Views with an underscore (e.g., `_Layout.cshtml`, `_Navigation.cshtml`) to keep your project organized and clear.
