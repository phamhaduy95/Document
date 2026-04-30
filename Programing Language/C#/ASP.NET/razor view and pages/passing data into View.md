# Passing Data to Razor Views

In ASP.NET Core, there are three primary mechanisms for passing data from a Controller to a View: **Strongly-Typed Models**, **ViewData**, and **ViewBag**.

---

## 1. Strongly-Typed Models (Recommended)
This is the standard and most robust approach. It involves passing a specific C# object (often called a **ViewModel**) to the view.

### Why use it?
- **IntelliSense**: Full code completion in your `.cshtml` files.
- **Type Safety**: Errors are caught at compile-time rather than runtime.
- **Refactoring Support**: Changing a property name in the model updates the view (or shows an error).

### Example:
**1. Define the ViewModel**
```csharp
public class UserProfileViewModel
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```

**2. Pass the Model from the Controller**
```csharp
public IActionResult Details()
{
    var model = new UserProfileViewModel { Name = "John", Age = 30 };
    return View(model);
}
```

**3. Receive the Model in the View**
```cshtml
@model UserProfileViewModel

<h2>User: @Model.Name</h2>
<p>Age: @Model.Age</p>
```

---

## 2. ViewData
`ViewData` is a dictionary object (`ViewDataDictionary`) accessed via string keys. It is useful for passing small amounts of data that aren't part of the main model (like a page title).

- **Controller**: `ViewData["Title"] = "Home Page";`
- **View**: `<h1>@ViewData["Title"]</h1>`

> [!IMPORTANT]
> `ViewData` requires **explicit casting** in the view if you are passing complex objects, as it stores everything as an `object`.

---

## 3. ViewBag
`ViewBag` is a dynamic property that acts as a wrapper around `ViewData`. It allows you to use dot notation instead of string keys.

- **Controller**: `ViewBag.Message = "Succesfully saved!";`
- **View**: `<div class="alert">@ViewBag.Message</div>`

### Limitations:
- **No IntelliSense**: You won't get any help from the IDE when typing property names.
- **Runtime Errors**: If you mistype a property name, the app will crash at runtime rather than failing to compile.

---

## 4. Which One Should You Use?

| Feature | Strongly-Typed Model | ViewData / ViewBag |
| :--- | :--- | :--- |
| **Best For** | Primary data, Forms, Large objects | Small data, Metadata (Titles), Layout info |
| **Pros** | Type-safe, IntelliSense, Clean code | Quick, No class definition needed |
| **Cons** | Requires a class definition | Prone to typos, No IntelliSense |

> [!TIP]
> Use **Strongly-Typed Models** for 95% of your work. Reserve `ViewData` for passing data into Layouts (like setting the `<title>` tag) or for very small, one-off messages.
