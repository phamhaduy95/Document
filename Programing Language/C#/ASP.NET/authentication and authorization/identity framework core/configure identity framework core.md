# Configuring ASP.NET Core Identity

To use ASP.NET Core Identity, you must register the required services and configure the security options in your `Program.cs` file. This involves connecting Identity to your database and defining your security policies.

---

## 1. Database Configuration
Identity depends on Entity Framework Core to store user data. First, register your `DbContext` and specify the database provider.

```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## 2. Registering Identity Services
Use the `AddIdentity<TUser, TRole>` or `AddIdentityCore<TUser>` method to register the identity system. The configuration lambda allows you to set your application's security policies.

```csharp
builder.Services.AddIdentity<IdentityUser, IdentityRole>(options => 
{
    // Password settings
    options.Password.RequireDigit = true;
    options.Password.RequiredLength = 8;
    options.Password.RequireNonAlphanumeric = false;
    options.Password.RequireUppercase = true;

    // Lockout settings
    options.Lockout.DefaultLockoutTimeSpan = TimeSpan.FromMinutes(15);
    options.Lockout.MaxFailedAccessAttempts = 5;
    options.Lockout.AllowedForNewUsers = true;

    // User settings
    options.User.RequireUniqueEmail = true;
    options.User.AllowedUserNameCharacters = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789-._@+";

    // Sign-in settings
    options.SignIn.RequireConfirmedEmail = true;
})
.AddEntityFrameworkStores<AppDbContext>() // Link to EF Core
.AddDefaultTokenProviders();               // Required for token generation (email/reset)
```

---

## 3. Configuration Categories
The **`IdentityOptions`** object provides several sub-objects for specific categories:

### Password Policy (`options.Password`)
Defines the complexity requirements for user passwords.
- **`RequiredLength`**: Minimum characters (default: 6).
- **`RequireDigit`**: Must contain at least one number (default: true).
- **`RequireNonAlphanumeric`**: Must contain at least one symbol (default: true).

### User Policy (`options.User`)
Defines rules for user account metadata.
- **`RequireUniqueEmail`**: Ensures that no two users share the same email address.
- **`AllowedUserNameCharacters`**: Restricts the characters allowed in a username.

### Lockout Policy (`options.Lockout`)
Configures protection against brute-force attacks.
- **`MaxFailedAccessAttempts`**: How many failed attempts are allowed before lockout (default: 5).
- **`DefaultLockoutTimeSpan`**: How long the account is locked (default: 5 minutes).

### Sign-In Policy (`options.SignIn`)
Defines the prerequisites for a successful sign-in.
- **`RequireConfirmedEmail`**: If true, users cannot log in until they verify their email.

---

## 4. Alternative Configuration (Options Pattern)
You can also configure specific parts of the Identity system separately using the standard .NET Options pattern.

```csharp
builder.Services.Configure<PasswordOptions>(options =>
{
    options.RequiredLength = 20; // Enforce very long passwords
    options.RequireDigit = true;
});
```
