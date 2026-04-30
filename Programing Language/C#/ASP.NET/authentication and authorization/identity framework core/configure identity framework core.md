Configure Identity Framework Core

when you add Identity framework core in your project, two configuration
steps must be done for Identity Framework core to run properly.

/\*\* add IdentityDbContext and define some EF configuration options for
project \*/

builder.Services.AddDbContext\<ToDoAppDbContext\>(options =\>

options.UseSqlServer(builder.Configuration.GetConnectionString(\"DefaultConnection\")));

/\*\* add Identity Framework core and its settings\*/

/\*\* add Identity Framework core and its settings\*/

builder.Services.AddIdentityCore\<User\>(options =\> {

options.Stores.MaxLengthForKeys = 250;

options.SignIn.RequireConfirmedAccount = true;

options.Password.RequiredLength = 20;

options.User.RequireUniqueEmail =false;

options.Lockout.AllowedForNewUsers = true;

}).AddEntityFrameworkStores\<ToDoAppDbContext\>();

Adding default configuration

The AddIdentityCore method is used for defining setting for Identity
framework core. The input lambda function exposes IdentityOption object,
from which we can provides some basic setting for these Identity
properties:

- User: This property is used to configure the username and email
  options for user accounts using the UserOptions class.

- SignIn: This property is used to specify the confirmation requirements
  for accounts using the SignInOptions class.

- Password: This property is used to define the password policy using
  the PasswordOptions class.

- LockOut: This property uses the LockoutOptions class to define the
  policy for locking out accounts after a number of failed attempts to
  sign in.

Each of these properties also contains more details option to setups.
Some important ones are:

- User.AllowedUserNameCharacters specifies the characters allowed in
  usernames. The default value is the set of upper and lowercase A--Z
  characters, the The default value is false.

- Password.RequiredLength specifies a minimum number of characters for
  passwords. The default value is 6.

- Password.RequiredUniqueChars specifies the minimum number of unique
  characters that password must contain. The default value is 1.

- Password.RequireNonAlphanumeric This property specifies whether
  passwords must contain nonalphanumeric characters, such as punctuation
  characters. The default value is true.

- Password.RequireLowercase specifies whether passwords must contain
  lowercase characters. The default value is true.

- Password.RequireUppercase specifies whether passwords must contain
  uppercase characters. The default value is true.

- Password.RequireDigit specifies whether passwords must contain number
  characters. The default value is true.

- SignIn.RequireConfirmedEmail, when is set to true, only accounts with
  confirmed email addresses can sign in. The default value is false.

- SignIn.RequireConfirmedPhoneNumber when is set to true, only accounts
  with confirmed phone numbers can sign in. The default value is false.

- LockOut.MaxFailedAccessAttempts specifies the number of failed
  attempts allowed before an account is locked out. The default value is
  5.

- LockOut.DefaultLockoutTimeSpan specifies the duration for lockouts.
  The default value is 5 minutes.

- LockOut.AllowedForNewUsers determines whether the lockout feature is
  enabled for new accounts. The default value is true.

you can also configurate any property of Identity framework core using
option pattern. For example:

/\*\* you can add configuration for any property of Identity framework
core directly using Option pattern \*/

builder.Services.Configure\<PasswordOptions\>(options =\>

{

options.RequiredLength = 20;

options.RequireDigit = true;

})
