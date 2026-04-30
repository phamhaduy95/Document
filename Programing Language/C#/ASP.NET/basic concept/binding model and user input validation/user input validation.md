# Model Validation in ASP.NET Core

Data sent by clients can be unpredictable or malicious. Model validation ensures that the data your application receives conforms to specific rules and prerequisites before you process it.

---

## 1. When Validation Occurs
Validation occurs **after** model binding but **before** the action method or page handler executes. However, the handler will always execute even if validation fails, so you must check the result manually within the handler.

---

## 2. Using Validation Attributes
Validation attributes allow you to specify rules directly on your model properties using declarative metadata.

| Attribute | Description |
| :--- | :--- |
| **`[Required]`** | Ensures the property is not null or empty. |
| **`[StringLength]`** | Sets the maximum (and optionally minimum) length of a string. |
| **`[Range]`** | Ensures a numeric value falls within a specific range. |
| **`[EmailAddress]`** | Validates that the value has a correct email format. |
| **`[Phone]`** | Validates that the value has a correct phone number format. |
| **`[RegularExpression]`** | Validates the input against a custom regex pattern. |

### Example ViewModel
```csharp
public class UserViewModel 
{
    [Required]
    public Guid Id { get; set; }

    [StringLength(100, ErrorMessage = "Name length cannot exceed 100 characters.")]
    public string Name { get; set; }

    [Range(0, 120)]
    public int Age { get; set; }

    [EmailAddress]
    public string Email { get; set; }
}
```

---

## 3. Checking Validation Results
The result of the validation attempt is stored in the **`ModelState`** object. In your action method, you should check the `ModelState.IsValid` property.

```csharp
[HttpPost("student")]
public IActionResult AddStudent(UserViewModel user)
{
    if (!ModelState.IsValid) 
    {
        // If any property fails validation, return a 400 Bad Request
        return BadRequest(ModelState);
    }

    // If we reach here, the data is valid
    return Ok(user);
}
```

> [!NOTE]
> If your controller is decorated with the **`[ApiController]`** attribute, the framework automatically performs this check and returns a `400 Bad Request` if validation fails, allowing you to omit the manual `if (!ModelState.IsValid)` block.

---

## 4. Custom Validation Attributes
For validation logic that isn't covered by built-in attributes, you can create a custom attribute by inheriting from `ValidationAttribute` and overriding the `IsValid` method.

```csharp
public class MyCustomValidationAttribute : ValidationAttribute
{
    protected override ValidationResult IsValid(object value, ValidationContext validationContext)
    {
        // Add your custom validation logic here
        return ValidationResult.Success;
    }
}
```
