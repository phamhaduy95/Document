# Customizing ASP.NET Core Identity

ASP.NET Core Identity is designed to be highly extensible. You can easily add custom properties to your users, change the type of their primary keys, or even rename the underlying database tables.

---

## 1. Adding Custom Properties
The default `IdentityUser` class only includes basic fields like `Email`, `PhoneNumber`, and `PasswordHash`. To store additional information (such as a user's name or date of birth), create a custom class that inherits from `IdentityUser`.

```csharp
public class AppUser : IdentityUser
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public DateTime CreatedAt { get; set; } = DateTime.UtcNow;
}
```

### Update the DbContext
You must update your `DbContext` to recognize the custom user class.

```csharp
public class AppDbContext : IdentityDbContext<AppUser>
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }
}
```

---

## 2. Changing the Primary Key Type
By default, Identity uses `string` (storing a GUID) for the `Id` property. You can change this to `int`, `long`, or `Guid` by specifying the type in the inheritance chain.

```csharp
// Use Guid as the Primary Key type
public class AppUser : IdentityUser<Guid> { }
public class AppRole : IdentityRole<Guid> { }

// Update the DbContext signature
public class AppDbContext : IdentityDbContext<AppUser, AppRole, Guid>
{
    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }
}
```

---

## 3. Customizing Table Names
If you prefer not to use the default `AspNet...` table names, you can rename them in the `OnModelCreating` method using the Fluent API.

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder); // Always call base first!

    builder.Entity<AppUser>().ToTable("Users");
    builder.Entity<AppRole>().ToTable("Roles");
    builder.Entity<IdentityUserRole<string>>().ToTable("UserRoles");
    builder.Entity<IdentityUserClaim<string>>().ToTable("UserClaims");
}
```

---

## 4. Registering the Custom Classes
Ensure that your `Program.cs` is configured to use your custom classes instead of the defaults.

```csharp
builder.Services.AddIdentity<AppUser, IdentityRole>()
    .AddEntityFrameworkStores<AppDbContext>()
    .AddDefaultTokenProviders();
```

> [!IMPORTANT]
> Whenever you modify your user model or table configuration, you must run an **EF Core Migration** (`Add-Migration`) and update your database (`Update-Database`) for the changes to take effect.
