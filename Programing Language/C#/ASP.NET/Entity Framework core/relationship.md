# Entity Relationships in EF Core

Entity Framework Core allows you to define relationships between your entities using navigation properties and the Fluent API.

---

## 1. One-to-One Relationship
In a one-to-one relationship, a single record in one table is associated with exactly one record in another table.

```csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
    public BlogImage BlogImage { get; set; } // Navigation property
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    
    public int BlogId { get; set; } // Foreign Key
    public Blog Blog { get; set; }  // Navigation property
}
```

### Fluent API Configuration
```csharp
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(i => i.BlogId);
```

---

## 2. One-to-Many Relationship
One record in the principal table can be associated with many records in the dependent table. This is the most common relationship type.

```csharp
public class Blog
{
    public int BlogId { get; set; }
    public ICollection<Post> Posts { get; set; } // Collection navigation
}

public class Post
{
    public int PostId { get; set; }
    public int BlogId { get; set; } // Foreign Key
    public Blog Blog { get; set; }  // Reference navigation
}
```

---

## 3. Many-to-Many Relationship
Many-to-many relationships require collection navigation properties on both sides. EF Core can automatically discover these and manage the hidden "join table" for you.

```csharp
public class Post
{
    public int PostId { get; set; }
    public ICollection<Tag> Tags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }
    public ICollection<Post> Posts { get; set; }
}
```

### Custom Join Entity
If you need to store additional data on the relationship (e.g., `DateAdded`), you should define a bespoke join entity type.

```csharp
modelBuilder.Entity<Post>()
    .HasMany(p => p.Tags)
    .WithMany(t => t.Posts)
    .UsingEntity<PostTag>(
        l => l.HasOne(pt => pt.Tag).WithMany().HasForeignKey(pt => pt.TagId),
        r => r.HasOne(pt => pt.Post).WithMany().HasForeignKey(pt => pt.PostId),
        j => {
            j.HasKey(pt => new { pt.PostId, pt.TagId }); // Composite Key
        });
```

---

## 4. Delete Behaviors
Delete behaviors determine what happens to dependent entities when a principal entity is deleted.

- **Cascade**: Dependent entities are deleted automatically. (Default for required relationships).
- **ClientSetNull / SetNull**: The foreign key of the dependent entities is set to `null`. (Requires the foreign key to be nullable).
- **Restrict**: Deletion of the principal is prohibited if any dependents exist.

```csharp
modelBuilder.Entity<Post>()
    .HasOne(p => p.Blog)
    .WithMany(b => b.Posts)
    .IsRequired() // Required relationship triggers Cascade by default
    .OnDelete(DeleteBehavior.Cascade);
```
