1.  Introduction

The Identity Framework core is the ASP.NET extension package, produced
by Microsoft, whose purpose is to provide the viable user storage for
supporting the authentication and authorization process. The Identity
Framework core offers various time-saving and convenient way to build
variety of features such as:

- Managing Database schema for storing user and claims.

- Creating a user in the database.

- Password validation and rules.

- Handling user account lockout.

- Saving additional claims into database.

As the user storage or database, the Identity Framework core is built on
the solid foundation of EF core. As result, identity framework core
consists of two important modules. The first is the DbContext class and
Entity Models to generate the database, the second is the APIs for
querying or modifying user data inside database.

Apart from the main usage as the user store provider, the Identity
Framework core also inject its own authentication scheme to the
authentication middleware and make it as the default scheme. For that
reason, you don't need to call AddAuthentication method explicitly.

> ***Note**: The cookie authentication scheme is included by default.*

Entity framework core module

Identity framework core integrates the EF core by fault as the main ORM
library to manage the database for storing user data. The EF core
implementation includes the IdentityDbContext class and the several
built-in entity models each of which is responsible for one table in
database. They are:

IdentityUser manages *AspNetUsers* table which consists user info such
as PhoneNumber, UserName, Id, Email, ...

IdentityUserClaim manages *AspNetUserClaims* which holds user claims.

IdentityUserLogin and IndentityUserToken represents the
*AspNetUserLogins* table and *AspNetUserTokens* table respectively which
is used to setup the third-party logins services (for example login to
your app using Google and Facebook account).

IdentityRole, IdentityUserRole and IdentityRoleClaims handle
*AspNetUserRoles, AspNetRoles,* and *AspNetRoleClaims*. These table is
used mostly to build the legacy role-based authentication model.

Just like any regular entity project, you also need to create the
migration file and generate the table in database using
Microsoft.EntityFrameworkCore.Tools package. After doing so, you can
examine every generated table by Identity framework core in database in
your SQL management like the picture below.

![Contextual menu on AspNetUsers table in SQL Server Object
Explorer](media/image1.png){width="3.6694444444444443in"
height="5.904166666666667in"}

The sample code below will guide you to set up the EF core part for your
Identity framework core:

The first step is to define the DbContext class which extends the
IdentityDbcontext. Although you can use IdentityDbContext directly, it
is recommended to create brand new class from IdentityDbContext as it is
highly likely to have additional tables for storing different data in
your projects.

using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

using Microsoft.EntityFrameworkCore;

public class ToDoAppDbContext : IdentityDbContext\<IdentityUser\>

{

public DbSet\<ToDoAction\> ToDoAction;

// you don't need to override the Unconfigure method to set the
configuration like we often do with regular DbContext class. The
configuration options are passed through the class constructor instead.

public ToDoAppDbContext(DbContextOptions options): base(options)

{

}

// if you have other tables beside the default tables provided by
Identity Framework Core, you still need add some proper table
configuration for these tables.

protected override void OnModelCreating(ModelBuilder builder)

{

> builder.ApplyConfiguration(new ToDoActionConfiguration());
>
> builder.Seed();

base.OnModelCreating(builder);

}

}

Sometimes, it is necessary to provide some initial set of users and
roles for your app such as admin user. To do so, create the extension
method for ModelBuilder class Seed and call it inside OnModelCreating.

using Microsoft.AspNetCore.Identity;

using Microsoft.EntityFrameworkCore;

public static class ModelBuilderExtension

{

public static void Seed(this ModelBuilder builder)

{

// create initial role admin.

> var roleId = Guid.NewGuid().ToString();

builder.Entity\<IdentityRole\>().HasData(new

{

RoleName = \"admin\",

RoleDescription = \"\",

RoleId = roleId

}); ;

// create the first user admin

var userId = Guid.NewGuid().ToString();

var hasher = new PasswordHasher\<User\>();

builder.Entity\<User\>().HasData(new User

{

UserName = \"admin\",

NormalizedUserName = \"admin\",

Email = \"TodoAppAdmin@gmail.com\",

NormalizedEmail = \"ToDoAppAdmin@gmail.com\",

Id = userId,

PhoneNumber = \"01257845621\",

PasswordHash = hasher.HashPassword(null,\"123456\"),

SecurityStamp = String.Empty,

});

// add user admin to admin role

builder.Entity\<IdentityUserRole\<string\>\>().HasData(new

{

userId = userId,

roleId = roleId,

});

}

}

2.  Identity API

the Identity API consists of several built-in DI services for querying
or modifying the data from the database generated by Identity Framework
(These DI are automatically injected when you add Identity framework
core through configuration in program.cs).

- UserManager\<IdentityUser\>: DI service for handling user data.

- SignInManager: DI service which provides several API for handling
  sign-in task

- RoleManager\<IdentityRole\>: DI service for handling role data.

3.1 UserManager and RoleManager

both UserManager and RoleManager services three main methods for
manipulating data. These three are:

- CreateAsync()

- UpdateAsync()

- DeleteAsync()

These data manipulation methods return the IdentityResult object which
represents the result from these three operations. For this object, we
can example whether the operation is executed correctly or not using
Successed (bool) property. In case the operation fails, we can acquire
the error info through Errors (Inumerable\<IdentityError\>) property.

Beside these data manipulation method, there are a handful of useful
methods for query data. For example.

For UserManger\<TUser\> service, there are:

- Task\<TUser\> FindByIdAsync(string userId): acquire a instance of User
  from database whose Id is exact to the required userId provided. This
  is the most common method for searching user.

- Task\<TUser\> FindByEmailAsync(string email): do same as the method
  above but using Email as the hint for searching. This method is
  practical only the

- UserManager.Users: return all user instances contained in database if
  any. This is a regular property of UserManager not method.

- Task\<IList\<TUser\>\> GetUsersInRoleAsync(string roleName): get list
  of users who are in the targeted role.

The example below will demonstrate the typical workflow for using
Identity API. The code from this example is to define the DI service for
managing user or account data for application.

Like regular DI service, the first thing is to create the Interface for
the Service.

public interface IUserService

{

public Task\<ResponseResult\> AddUser(UserViewModel model);

public Task\<ResponseResult\> UpdateUser(UserViewModel model);

public Task\<ResponseResult\> DeleteUser(string userId);

}

Note: The ResponseResult is the helper class to hold status code and
message.

The next step is to create concreate class implementing the DI service
interface above.

public class UserService : IUserService

{

/\*\* register the UserManager and SignInManager DI service\*/

private readonly UserManager\<User\> \_userManager;

private readonly SignInManager\<User\> \_signInManager;

> public UserService(UserManager\<User\> userManager,
> SignInManager\<User\> signInManager)

{

\_userManager = userManager;

\_signInManager = signInManager;

}

/\*\* implement AddUser method \*/

public async Task\<ResponseResult\> AddUser(UserViewModel model)

{

var user = await \_userManager.FindByIdAsync(model.Id);

if (user != null)

return new ResponseResult(400, \"user Id already exist\");

/\*\* initiate PasswordHasher object which is the factory that can
generate hash code from string \*/

> var hasher = new PasswordHasher\<User\>();

User newUser = new User

{

UserName = model.UserName,

Id = model.Id,

Email = model.Email,

PasswordHash = hasher.HashPassword(null, model.Password),

PhoneNumber = model.PhoneNumber,

};

/\*\* use createAsync method to add new record of user to the database
\*/

var result = await \_userManager.CreateAsync(newUser);

if (result.Succeeded)

{

return new ResponseResult(201, \"successfully create new account\");

}

else

{

return new ResponseResult(400, \"can not add new user due to database
connect\");

}

}

public async Task\<ResponseResult\> DeleteUser(string userId)

{

var user = await \_userManager.FindByIdAsync(userId);

if (user != null)

{

var response = await \_userManager.DeleteAsync(user);

return new ResponseResult(200, \"successfully delete user\");

}

return new ResponseResult(404, \"userId not found\");

}

public async Task\<ResponseResult\> UpdateUser(UserViewModel model)

{

var user = await \_userManager.FindByIdAsync(model.Id);

if (user != null)

{

var userToUpdate = new User

{

UserName = model.UserName,

Email = model.Email,

Id = model.Id,

PhoneNumber = model.PhoneNumber,

PasswordHash = new PasswordHasher\<User\>().HashPassword(user,
model.Password),

};

var result = await \_userManager.UpdateAsync(userToUpdate);

if (result.Succeeded)

{

return new ResponseResult(200, \"successfully update user data\");

}

return new ResponseResult(400, \"can not update user data due to
database error\");

}

return new ResponseResult(404, \"user Id not found to be updated\");

}

}

3.2 SignInManager

SignInManager consists of two main methods for executing *sign-in* *and
sign-out* operations for user: PasswordSignInAsync and

SignInResult PasswordSignInAsync(username, password, persist, lockout):
This method signs the user into the application with the specified
username and password. The persist argument specifies whether the
authentication cookie persists after the browser is closed. The lockout
argument specifies whether a failed sign-in attempt counts toward a
lockout. This method also has an overload version which use IdentityUser
user instead of username string value.

- The SignInResult returned from PasswordSignInAsync indicates the
  results of sign-in operation. you can check these properties from the
  SignInResult Object to acquire more details:

- Succeeded: This property returns true if the user has been
  successfully signed into the application.

- IsLockedOut: This property returns true if the user is currently
  locked out. See Chapter 9 for details of how lockouts work.

- RequiresTwoFactor: This property returns true if two-factor
  authentication is required. See Chapter 11 for details of creating
  workflows for two-factor authentication.

- IsNotAllowed: This property returns true if the user is not allowed to
  sign in. This is most commonly the case when the email address has not
  been confirmed.

Example:

public async Task\<IActionResult\> OnPostAsync(string returnUrl = null)

{

returnUrl = returnUrl ?? Url.Content(\"\~/\");

if (ModelState.IsValid)

{

// This doesn\'t count login failures towards account lockout

// To enable password failures to trigger account lockout,

// set lockoutOnFailure: true

var result = await \_signInManager.PasswordSignInAsync(Input.Email,

Input.Password, Input.RememberMe, lockoutOnFailure: true);

if (result.Succeeded)

{

\_logger.LogInformation(\"User logged in.\");

return LocalRedirect(returnUrl);

}

if (result.RequiresTwoFactor)

{

return RedirectToPage(\"./LoginWith2fa\", new

{

ReturnUrl = returnUrl,

RememberMe = Input.RememberMe

});

}

if (result.IsLockedOut)

{

\_logger.LogWarning(\"User account locked out.\");

return RedirectToPage(\"./Lockout\");

}

else

{

ModelState.AddModelError(string.Empty, \"Invalid login attempt.\");

return Page();

}

}

// If we got this far, something failed, redisplay form

return Page();

}

SignOutAsync method signs the current user out of the application.

public async Task\<IActionResult\> OnPost(string returnUrl = null)

{

await \_signInManager.SignOutAsync();

\_logger.LogInformation(\"User logged out.\");

if (returnUrl != null)

{

return LocalRedirect(returnUrl);

}

else

{

return RedirectToPage();

}

}

Note: When the SignInAsync method return succeed result, for traditional
cookie authentication, the Identity Framework core automatically add any
credential data into cookie. When SignOutAsync is executed successfully,
all previous credential stored in cookie will be deleted, thus left the
client who make that request unauthenticated.

Adding Identity Framework core to the project

reading the "[*configure Identity Framework
core*](configure%20identity%20framework.docx)" document for more
details.
