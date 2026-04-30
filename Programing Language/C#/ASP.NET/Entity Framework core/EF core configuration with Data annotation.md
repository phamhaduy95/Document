# EF Core Configuration with Data Annotations

Data Annotations are attributes applied directly to your entity classes and properties to define how they should be mapped to the database. This approach is highly readable because the configuration lives alongside the data it describes.

---

## 1. Data Annotations vs. Fluent API

| Aspect | Data Annotations | Fluent API |
| :--- | :--- | :--- |
| **Simplicity** | Very high; easy to implement. | Lower; requires more code. |
| **Placement** | Directly on the entity class. | Centralized in `OnModelCreating`. |
| **Power** | Basic (Keys, Length, Tables). | Advanced (Indexes, Shadows, Complex Relationships). |
| **Decoupling** | Low (Models depend on EF namespaces). | High (Keeps domain models clean). |

> [!TIP]
> For professional, large-scale projects, the **Fluent API** is generally preferred because it keeps your entity models "clean" and allows for more complex database configurations that aren't possible with attributes.

---

## 2. Common Data Annotation Attributes

### 2.1 Primary Keys (`[Key]`)
By default, EF Core looks for a property named `Id` or `[EntityName]Id`. Use the `[Key]` attribute if your primary key has a custom name.

```csharp
public class Order
{
    [Key]
    public int OrderNumber { get; set; }
}
```

### 2.2 Table and Column Customization
You can explicitly define the database table name and specific column properties.

```csharp
[Table("tbl_Students")]
public class Student
{
    public int Id { get; set; }

    [Column("student_name", TypeName = "varchar(100)")]
    public string Name { get; set; }
}
```

### 2.3 Validation and Nullability
- **`[Required]`**: Tells EF Core that the column cannot be null (`NOT NULL`).
- **`[StringLength]`**: Sets the maximum character limit for string columns.

```csharp
public class User
{
    [Required]
    [StringLength(50, MinimumLength = 3)]
    public string UserName { get; set; }
}
```

### 2.4 Foreign Keys (`[ForeignKey]`)
While EF Core is excellent at inferring relationships, you can use `[ForeignKey]` to explicitly link a property to a navigation property.

```csharp
public class Post
{
    public int PostId { get; set; }

    [ForeignKey("Author")]
    public int AuthorId { get; set; }

    public User Author { get; set; }
}
```

### 2.5 Excluding Properties (`[NotMapped]`)
If you have a property that should only exist in your application logic and **not** be created in the database, use `[NotMapped]`.

```csharp
public class Employee
{
    public string FirstName { get; set; }
    public string LastName { get; set; }

    [NotMapped]
    public string FullName => $"{FirstName} {LastName}";
}
```
