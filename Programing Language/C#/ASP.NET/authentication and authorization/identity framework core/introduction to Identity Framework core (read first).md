# Introduction to ASP.NET Core Identity

ASP.NET Core Identity is a comprehensive membership system designed to handle user authentication and authorization. It provides a robust set of tools for managing users, passwords, profile data, roles, claims, and security features like two-factor authentication (2FA) and account lockout.

---

## 1. Key Features
- **User Management**: Easily create, update, and delete users and their associated data.
- **Security by Default**: Implements secure password hashing, multi-factor authentication, and protection against brute-force attacks via account lockout.
- **EF Core Integration**: Built on top of Entity Framework Core, allowing you to use familiar patterns to manage your security database.
- **Automatic Integration**: Hooks directly into the ASP.NET Core authentication middleware, making it the default identity provider for most web apps.

---

## 2. The EF Core Foundation
Identity uses EF Core to persist security data. It comes with a predefined set of tables to store user and role information.

| Entity Class | Database Table | Purpose |
| :--- | :--- | :--- |
| **`IdentityUser`** | `AspNetUsers` | Stores basic user info (Username, Email, PasswordHash, etc.). |
| **`IdentityRole`** | `AspNetRoles` | Stores role definitions (e.g., "Admin", "Editor"). |
| **`IdentityUserClaim`** | `AspNetUserClaims` | Stores claims assigned to individual users. |
| **`IdentityUserRole`** | `AspNetUserRoles` | Maps users to their assigned roles (Many-to-Many). |

### Customizing the DbContext
To use Identity, your application's `DbContext` must inherit from **`IdentityDbContext<TUser>`**.

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;

public class AppDbContext : IdentityDbContext<IdentityUser>
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

    protected override void OnModelCreating(ModelBuilder builder)
    {
        base.OnModelCreating(builder); // Important: Identity needs its own mapping!
    }
}
```

---

## 3. The Identity APIs (Managers)
Identity provides high-level "Manager" classes that you can inject into your controllers or services via Dependency Injection.

### `UserManager<TUser>`
The primary service for managing user data.
- **`CreateAsync(user, password)`**: Hashes the password and saves the user to the database.
- **`FindByIdAsync(id)` / `FindByEmailAsync(email)`**: Methods to retrieve users.
- **`UpdateAsync(user)` / `DeleteAsync(user)`**: Standard CRUD operations.

### `SignInManager<TUser>`
Handles the logic for signing users in and out.
- **`PasswordSignInAsync()`**: Verifies the username and password and, if successful, issues the authentication cookie.
- **`SignOutAsync()`**: Clears the authentication cookie from the client.
- **`TwoFactorSignInAsync()`**: Handles the second stage of 2FA.

---

## 4. Handling Sign-In Results
When you call `PasswordSignInAsync`, it returns a **`SignInResult`** object which tells you exactly what happened:

- **`Succeeded`**: The user is now logged in.
- **`IsLockedOut`**: The account is temporarily disabled due to too many failed attempts.
- **`RequiresTwoFactor`**: The user's identity is verified, but they must provide a second authentication factor.
- **`IsNotAllowed`**: The user is valid, but forbidden from logging in (e.g., their email address hasn't been confirmed yet).

---

## 5. Next Steps
To begin using Identity in your project, you must first configure the services and the database connection in `Program.cs`. Refer to the **[Configure Identity Framework Core](configure%20identity%20framework%20core.md)** guide for detailed instructions.
