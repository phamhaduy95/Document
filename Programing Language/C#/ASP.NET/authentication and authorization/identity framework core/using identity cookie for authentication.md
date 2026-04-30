Identity framework core use cookie as the default authentication scheme
for storing user identifiers so you don't need to explicitly register a
cookie scheme using AddCookie. However, you can provide the
configuration options to alter the behavior of cookie. For example.

builder.Services.ConfigureApplicationCookie(options =\>

{

// Cookie settings

options.Cookie.HttpOnly = true;

options.ExpireTimeSpan = TimeSpan.FromMinutes(5);

options.LoginPath = \"/Identity/Account/Login\";

options.AccessDeniedPath = \"/Identity/Account/AccessDenied\";

options.SlidingExpiration = true;

});
