# Entity Properties in EF Core

In Entity Framework Core, the public properties of your entity classes are mapped to columns in the database. You can customize this mapping using Data Annotations or the Fluent API.

---

## 1. Included and Excluded Properties
By default, EF Core includes all public properties that have a getter and a setter in the database model.

### Excluding Properties
To prevent a property from being mapped to a database column, use the `[NotMapped]` attribute or the `.Ignore()` method in the Fluent API. This is useful for calculated properties that only exist in your code.

```csharp
public class User
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
    
    [NotMapped]
    public string FullName => $"{FirstName} {LastName}";
}
```

---

## 2. Primary Keys
EF Core convention automatically treats a property named `Id` or `{EntityName}Id` (case-insensitive) as the primary key.

- **Data Annotation**: Use `[Key]`.
- **Fluent API**: Use `builder.HasKey(u => u.UserId);`.

---

## 3. Column Names and Types
You can explicitly define the column name and the specific SQL data type.

### Data Annotations
```csharp
[Column("user_email", TypeName = "varchar(255)")]
public string Email { get; set; }
```

### Fluent API (Recommended)
```csharp
builder.Property(u => u.Email)
    .HasColumnName("user_email")
    .HasColumnType("varchar(255)")
    .IsRequired();
```

---

## 4. Generated Values
You can configure EF Core to let the database generate values for certain properties, such as identity columns or timestamps.

- **ValueGeneratedOnAdd**: The database generates a value when a new row is inserted (e.g., `IDENTITY` in SQL Server).
- **ValueGeneratedOnAddOrUpdate**: The database generates a new value whenever the row is updated (useful for `RowVersion` or `LastModified` columns).

```csharp
builder.Property(u => u.CreatedAt)
    .HasDefaultValueSql("GETDATE()")
    .ValueGeneratedOnAdd();
```
