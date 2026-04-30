MODEL VALIDATION

1.  **The important of data validation**

Data sent by client can sometimes be arbitrary and unpredictable. It is
not uncommon to set some prerequisites and strict rule for controlling
type and format of each piece of data. ASP.NET support data validation
features for binding model acquired from HTTP request.

Validation occurs in the Razor Pages framework after model binding, but
before\
the page handler executes.

![](media/image1.png){width="6.5in" height="4.725694444444445in"}

Model Validation is often used to check for non-malicious errors:

- Data should be formatted correctly. (e-mail format)

- Numbers might need to be in a particular range.

- Some values may be required but others are optional

- Values must conform to your business requirements

2.  Validation attributes

Validation attributes allow you to specify the rules that your binding
model should conform to. They provide metadata about your model by
describing the sort of data the binding model should contain, as opposed
to the data itself.

You can apply Validation attributes directly to your binding models to
indicate the type of data that's acceptable.

public class UserViewModel {

\[Required\] // value must be provided.

public Guid Id { get; set; }

> //The StringLength Attribute sets the maximum length for the property

\[StringLength(200)\]

public string Name { get; set;}

\[Range(0,200)\]

public int Age { get; set;}

// check this property has valid phone number format

\[Phone\]

public string TeleNumber { get; set;}

// verify whether a property has a valid email address format

\[EmailAddress\]

public string Email { get; set; }

}

Note: The validation attributes also have its application for validate
property of entity model class.

You can find the entire set of validation attributes in Microsoft
ASP.NET document
"<https://docs.microsoft.com/en-us/aspnet/core/mvc/advanced/custom-model-binding?view=aspnetcore-6.0>"

Validation attributes also let you specify the error message to be
displayed for invalid input.

\[StringLength(100, ErrorMessage = \"Name length can\'t be more than
100.\")\]

public string Name { get; set;}

3.  Checking validation

Validation of the binding model occurs before the page handler executes,
but note that the handler always executes, whether the validation failed
or succeeded. It's the responsibility of the page handler to check the
result of the validation. Validation happens automatically, but handling
validation failures is the responsibility of the page handler. ASP.NET
core stores the output of the validation attempt in ModelState object.

\[HttpPost(\"student\")\]

public IActionResult AddStudent(UserViewModel user){

> // If any property inside UserViewModel don't pass validation test,
> the ModelState.IsValid will be set false.

if (!ModelState.IsValid) {

return BadRequest();

}

Console.WriteLine(\$\" user {user.Id} name {user.Name}, age:
{user.Age}\");

return Ok(user);

}

4.  Creating custom validation attribute
