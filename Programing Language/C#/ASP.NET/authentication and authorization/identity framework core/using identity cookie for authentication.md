# Using Identity Cookies for Authentication

By default, ASP.NET Core Identity uses Cookies to maintain user sessions. While Identity handles the basics automatically, you can fine-tune the cookie's behavior to meet your security and user experience requirements.

---

## 1. Configuring Identity Cookies
You can customize the application cookie by calling **`ConfigureApplicationCookie`** in your `Program.cs`. This call must appear **after** you have registered the Identity services.

```csharp
builder.Services.ConfigureApplicationCookie(options =>
{
    // Basic Security
    options.Cookie.HttpOnly = true; // Prevents client-side scripts (XSS) from reading the cookie
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always; // Ensures the cookie is only sent over HTTPS

    // Expiration Logic
    options.ExpireTimeSpan = TimeSpan.FromDays(14); // How long the session lasts
    options.SlidingExpiration = true; // Renews the cookie's life if the user is active

    // Redirection Paths
    options.LoginPath = "/Account/Login";               // Redirect here for unauthenticated users
    options.AccessDeniedPath = "/Account/AccessDenied"; // Redirect here for unauthorized users
});
```

---

## 2. Key Configuration Options

| Property | Description |
| :--- | :--- |
| **`LoginPath`** | The URL where users are redirected if they try to access a protected page while not logged in. |
| **`AccessDeniedPath`** | The URL where users are redirected if they are logged in but do not have the required permissions (e.g., trying to access Admin pages as a regular user). |
| **`ExpireTimeSpan`** | The amount of time the authentication ticket remains valid. |
| **`SlidingExpiration`** | If enabled, the cookie's expiration timer is reset if a request is made when more than half of the `ExpireTimeSpan` has elapsed. |
| **`Cookie.HttpOnly`** | Set to `true` to mitigate XSS attacks by preventing JavaScript from accessing the cookie. |

---

## 3. Persistent vs. Session Cookies
When a user logs in via the **`SignInManager`**, the `isPersistent` parameter determines the type of cookie issued:

- **Session Cookie (`isPersistent: false`)**: The cookie is stored in the browser's memory and is typically deleted when the browser window is closed.
- **Persistent Cookie (`isPersistent: true`)**: The cookie is stored on the user's hard drive and remains valid even after the browser is closed, until the `ExpireTimeSpan` is reached. This is the "Remember Me" behavior.

```csharp
// Example login call with a persistent cookie
var result = await _signInManager.PasswordSignInAsync(
    Input.Email, 
    Input.Password, 
    isPersistent: true, // "Remember Me"
    lockoutOnFailure: false);
```
