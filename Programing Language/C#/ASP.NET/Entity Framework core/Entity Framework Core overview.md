# Entity Framework Core Overview

**Entity Framework (EF) Core** is an open-source, lightweight, and extensible version of the Entity Framework data access technology. It is an **Object-Relational Mapper (ORM)** that enables .NET developers to work with a database using .NET objects, eliminating the need for most manual data-access code.

---

## 1. Key Features
- **Code-First Approach**: Define your database schema using C# classes (entities). EF Core automatically handles the creation and updates of the database tables based on your code.
- **LINQ Integration**: Write type-safe database queries using standard C# LINQ syntax instead of raw SQL strings.
- **Cross-Platform**: Fully supports Windows, macOS, and Linux.
- **Migrations**: Provides a robust system to version and apply schema changes to your database as your application evolves.

---

## 2. Core Workflow (Code-First)
The standard workflow for integrating EF Core into your project involves several key steps:

### 1. Define Entity Classes
An entity class represents a table in your database, and each instance represents a row.
```csharp
public class Blog
{
    public Guid BlogId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    // Foreign Key and Navigation property
    public Guid CategoryId { get; set; }
    public Category Category { get; set; }
}
```

### 2. Create the DbContext
The `DbContext` is the primary class that coordinates Entity Framework functionality for a given data model. It represents a session with the database.
```csharp
public class AppDbContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Category> Categories { get; set; }

    public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        // Fluent API configuration
        modelBuilder.ApplyConfiguration(new BlogConfiguration());
    }
}
```

### 3. Configure the Model (Fluent API)
Use the Fluent API in `OnModelCreating` to define complex constraints, primary keys, and relationships that can't be handled by simple attributes.
```csharp
public class BlogConfiguration : IEntityTypeConfiguration<Blog>
{
    public void Configure(EntityTypeBuilder<Blog> builder)
    {
        builder.HasKey(b => b.BlogId);
        builder.Property(b => b.Title).IsRequired().HasMaxLength(200);
        
        // Define One-to-Many relationship
        builder.HasOne(b => b.Category)
               .WithMany(c => c.Blogs)
               .HasForeignKey(b => b.CategoryId)
               .OnDelete(DeleteBehavior.Cascade);
    }
}
```

---

## 3. Database Migrations
Migrations allow you to evolve your database schema incrementally without losing data.

1.  **Add Migration**: Generates code to update the database to match your current model.
    ```powershell
    Add-Migration InitialCreate
    ```
2.  **Update Database**: Applies any pending migrations to the database.
    ```powershell
    Update-Database
    ```

---

## 4. Seeding Data
You can populate your database with initial data (like roles or categories) using the `HasData` method.
```csharp
modelBuilder.Entity<Category>().HasData(
    new Category { CategoryId = Guid.NewGuid(), CategoryName = "Technology" }
);
```
