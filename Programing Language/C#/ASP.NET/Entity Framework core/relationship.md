Types of relationship

One-to-one relationship

Add navigation property for both entity class

public class Blog

{

public int BlogId { get; set; }

public string Url { get; set; }

public BlogImage BlogImage { get; set; }

}

public class BlogImage

{

public int BlogImageId { get; set; }

public byte\[\] Image { get; set; }

public string Caption { get; set; }

public int BlogId { get; set; }

public Blog Blog { get; set; }

}

Configure relationship in OnModelCreating Method

internal class MyContext : DbContext

{

public DbSet\<Blog\> Blogs { get; set; }

public DbSet\<BlogImage\> BlogImages { get; set; }

protected override void OnModelCreating(ModelBuilder modelBuilder)

{

modelBuilder.Entity\<Blog\>()

.HasOne(b =\> b.BlogImage)

.WithOne(i =\> i.Blog)

.HasForeignKey\<BlogImage\>(b =\> b.BlogForeignKey);

}

}

One-to-many-relationship

Many-to-many-relationship

Many-to-many relationships require a collection navigation property on
both sides. They will be discovered by convention like other types of
relationships.

public class Post

{

public int PostId { get; set; }

public string Title { get; set; }

public string Content { get; set; }

public ICollection\<Tag\> Tags { get; set; }

}

public class Tag

{

public string TagId { get; set; }

public ICollection\<Post\> Posts { get; set; }

}

It is common to apply configuration to the join entity type. This action
can be accomplished via UsingEntity.

modelBuilder

.Entity\<Post\>()

.HasMany(p =\> p.Tags)

.WithMany(p =\> p.Posts)

.UsingEntity(j =\> j.ToTable(\"PostTags\"));

 it\'s best to create a bespoke CLR type. When configuring the
relationship with a custom join entity type both foreign keys need to be
specified explicitly

internal class MyContext : DbContext

{

public MyContext(DbContextOptions\<MyContext\> options)

: base(options)

{

}

public DbSet\<Post\> Posts { get; set; }

public DbSet\<Tag\> Tags { get; set; }

protected override void OnModelCreating(ModelBuilder modelBuilder)

{

modelBuilder.Entity\<Post\>()

.HasMany(p =\> p.Tags)

.WithMany(p =\> p.Posts)

.UsingEntity\<PostTag\>(

j =\> j

.HasOne(pt =\> pt.Tag)

.WithMany(t =\> t.PostTags)

.HasForeignKey(pt =\> pt.TagId),

j =\> j

.HasOne(pt =\> pt.Post)

.WithMany(p =\> p.PostTags)

.HasForeignKey(pt =\> pt.PostId),

j =\>

{

j.Property(pt =\>
pt.PublicationDate).HasDefaultValueSql(\"CURRENT_TIMESTAMP\");

j.HasKey(t =\> new { t.PostId, t.TagId });

});

}

}

public class Post

{

public int PostId { get; set; }

public string Title { get; set; }

public string Content { get; set; }

public ICollection\<Tag\> Tags { get; set; }

public List\<PostTag\> PostTags { get; set; }

}

public class Tag

{

public string TagId { get; set; }

public ICollection\<Post\> Posts { get; set; }

public List\<PostTag\> PostTags { get; set; }

}

public class PostTag

{

public DateTime PublicationDate { get; set; }

public int PostId { get; set; }

public Post Post { get; set; }

public string TagId { get; set; }

public Tag Tag { get; set; }

}

Optional and required relationship

This setting affects the deleting behavior of database when the one row
of primary row is deleted. If set optional, then foreign key slot within
the dependent table is set to null. When being set to required, then the
database will execute the cascade delete which will delete any
dependency row.

protected override void OnModelCreating(ModelBuilder modelBuilder)

{

modelBuilder.Entity\<Post\>()

.HasOne(p =\> p.Blog)

.WithMany(b =\> b.Post

.IsRequired(); // make the relationship is optional

}

Setting primary entity as forein key to null will delete this row
completely.
