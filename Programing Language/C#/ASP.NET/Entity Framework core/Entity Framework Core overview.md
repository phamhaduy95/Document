**Entity framework core**

1.  **Introduction**

Entity framework (EF) core is open-source library whose purpose is to
help .NET developer accessing databases using only C#. It is designed as
ORM (object relation mapping) which abstracts the database table as the
class and allow developer integrate the OOP paradigm to interact with
database using object-oriented way.

The EF core has some powerful features:

- Code-first approach allow us to define the table entity as class and
  automatically create or update data definition instead of creating the
  table in database server first then declare matching entity class
  later.

![](media/image1.png){width="5.90625in" height="1.34375in"}

- Using LINQ, which has many similarities with SQL statement, as the
  standard query language for EF core.

2.  **Pros and cons**

> The main benefits which EF core offers is the fast and simple
> development cycle. However, since EF core serves as high-level
> abstraction for database access, there are some overheads for EF core
> to be functional, which may lower performance. This issue can be
> solved with more low-level api such as ADO.NET when you want to
> improve the performance for critical operation.

3.  **Install EF core**

Although, EF core is developed and maintained by Microsoft, it is a
stand-alone library from standard .Net core. You can install EF core
into your project through NuGet packet manager.

4.  **Integrating EF core into your project**

In this section, we will demonstrate the regular workflow of EF core
using code-first approach, which includes several steps:

1.  Constructing entity class.

2.  creating Configuration for Dbcontext.

3.  Defining Dbcontext class.

4.  Populating the database with some initial rows.

5.  Creating migration through NUGET packet manager terminal.

All these steps can be grouped into one Module, which is often named as
DataAccessLayer. The module helps separating the database access
responsibility from the main program.

![](media/image2.png){width="6.5in" height="1.8583333333333334in"}

**3.1 Constructing the entity class**

The entity class is equivalent to table row in database, it consists of
public data fields which represent the columns within one row. You can
assume the entity class declaration is similar to the writing CREATE
TABLE statement in SQL.

During the entity class construction, we must:

- Define the entity 's data fields, which represent the columns of table
  in database.

- Define the one-many, one-one, many-many relationships between each
  entity.

- Use data annotation to add configuration to the entity. (optional)

The entity class may consist of navigation data field which will not be
generated in the final result table but is used to specify the one-one,
one-many, many-many relations with other entity.

internal class Blog

{

public Guid BlogsID { get; set; }

public string BlogsName { get; set; }

public string Content { get; set; }

public Guid CategoryID { get; set; }

// the navigation data field specify the connection between Blog and
Category

public Category Category { get; set; }

}

internal class Category

{

public Guid CategoryID { get; set; }

public string CategoryName { get; set; }

> // Navigation data field indicates that one Category record can be
> used by many Blog.

public ICollection\<Blogs\> Blogs { get; set; }

}

**3.2 Setting up the Configuration for DbContext**

Configuration for DbContext includes these tasks:

- adding constraints for entity data field.

- declaring primary key, foreign key

- setup the type of columns in database.

- adding index.

....

There are two ways to configure the DbContext in EF core: The first is
data annotations which is simple but quite limited while the second is
the fluent API which is more advanced and provides more option of
settings. In the context of this document, the fluent API is chosen to
demonstrate how to set the configuration for DbContext.

EF core contains IEnityTypeConfiguration generic interface which accept
the Entity class as the target for configuration. We subclass from that
interface and re-implement the Configure method whose input argument
EntityTypeBuilder contains set of useful method

public class BlogsConfiguration : IEntityTypeConfiguration\<Blogs\>

{

public void Configure(EntityTypeBuilder\<Blogs\> builder){

> // set BlogId as the primary key

builder.HasKey(b =\> b.BlogID);

// make BlogId value not nullable

> builder.Property(b =\> b.BlogID).IsRequired();

// set type of this column in database

> builder.Property(b =\> b.Content).HasColumnType(\"ntext\");
>
> // equivalent to VARCHAR(200);

builder.Property(b =\> b.Title).HasMaxLength(200);

builder.Property(b =\> b.Summary).HasMaxLength(200);

> // define the one-many relations between blogs and categories table

builder.HasOne(b =\> b.Category)

.WithMany(c =\> c.Blogs)

> .HasForeignKey(b =\> b.CategoryID) // define the one-many relations
> between blogs and categories table.

.HasConstraintName(\"FK_Categories\");

}

}

public void Configure(EntityTypeBuilder\<Categories\> builder){

builder.HasKey(c =\> c.CategoryID);

builder.Property(c =\> c.CategoryID).IsRequired();

builder.Property(c =\> c.CategoryName).HasMaxLength(200);

}

**3.3 Creating DbContext**

The
[DbContext](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext?view=efcore-2.0)
class is an integral part of Entity Framework. An instance of DbContext
represents a session with the database which can be used to query and
save instances of your entities to a database.

To use DbContext in our application, we need to create the class that
derives from DbContext, also known as context class.

There are three things we need to do when defining DbContext class:

- add list of DbSet which represents an entity set that can be used for
  create, read, update, and delete operations. You can treat DbSet as
  the Database table.

- Override the OnModelCreating and supply the configuration for each
  entity to the ModelBuilder object.

- Provide connection setting for DbContext in OnConfiguring method.

- Initiate some rows in tables through user-defined Seed extended method
  for ModelBuilder class (optional)

public class AppDbContext : DbContext {

// define two DbSet properties representing Blogs table and Categories
table in database.

> public DbSet\<Blogs\> Blogs;

public DbSet\<Categories\> Categories;

private string ConnectionString;

public AppDbContext(string ConnectionString):base(){

> this.ConnectionString = ConnectionString;
>
> }

// add connection setting to DbContext.

> protected override void Unconfiguring(DbContextOptionsBuilder
> optionsBuilder){
>
> // UseSqlServer() require proper connection string to be able to
> connect to SQL server.
>
> optionsBuilder.UseSqlServer(this.ConnectionString);
>
> }

// add configuration for Blog entity and Category entity.

protected override void OnModelCreating(ModelBuilder modelBuilder){

> modelBuilder.ApplyConfiguration(new BlogsConfiguration());
>
> modelBuilder.ApplyConfiguration(new CategoriesConfiguration());
>
> }
>
> }

The connection string is essential to Accessing Database through .Net
program as it contains information about database server address, userId
and password for login and settings for Db connection. It is often
stored in a configuration file in JSON format called appsettings.json to
avoid recompilation when its string value is required to be changed. For
example:

{

\"ConnectionStrings\": {

> \"DefaultConnection\": \"Data Source=DESKTOP-8CIII70;Initial
> Catalog=demo;User
> Id=sa;Password=123456;MultipleActiveResultSets=true;\"

}

}

To get the value of the connection string from JSON file, you can use
ConfigurationBuilder to parse the configuration settings contained in
JSON file.

To able to access all utilities of ConfigurationBuilder, you should
include these dependencies into your project:

- Microsoft.Extensions.Configuration (base package)

- Microsoft.Extensions.Configuration.FileExtensions ( add some extension
  method to access file in local file system manager )

- Microsoft.Extensions.Configuration.Json (allow working with JSON file)

Example:

IConfigurationRoot configuration = new ConfigurationBuilder()

.SetBasePath(Directory.GetCurrentDirectory())

.AddJsonFile(\"appsettings.json\")

.Build();

// get one of connectionString value.

string connectionString =
configuration.GetConnectionString(\"DefaultConnection\");

You can also apply Factory pattern for initiating DbContext object, the
Microsoft.EntityFrameworkCore.Design packet provides it developers
IDesignTimeDbContextFactory interface to create the DbContext factory
class.

public class AppDbContextFactory :
IDesignTimeDbContextFactory\<AppDbContext\> {

public AppDbContext CreateDbContext(string\[\] args){

> IConfigurationRoot configuration = new ConfigurationBuilder()
>
> .SetBasePath(Directory.GetCurrentDirectory())
>
> .AddJsonFile(\"appsettings.json\")
>
> .Build();
>
> string connectionString =
> configuration.GetConnectionString(\"DefaultString\");

return new AppDbContext(connectionString);

}

}

**3.4 Populate the database with some initial records. (optional)**

you can add some initial records to the table when its is first
generated in database. The EntityModelBuilder exposed in OnModelCreating
contains Entity\<\>() from which rows can be added. For example.

modelBuilder.Entity\<Category\>().HasData(new Category(){

CategoryID = Guid.Parse(\"7C318165-4FD6-4FE2-A85C-069B08AE611F\"),

CategoryName = \"News\",

Blogs = new List\<Blog\>()

});

You can use method extension for EntityModelBuilder

public static void Seed(this ModelBuilder modelBuilder){

modelBuilder.Entity\<Category\>().HasData(new Category(){

> CategoryID = Guid.Parse(\"7C318165-4FD6-4FE2-A85C-069B08AE611F\"),

CategoryName = \"News\",

Blogs = new List\<Blog\>()

});

modelBuilder.Entity\<Blog\>().HasData(new Blog(){

BlogID = Guid.NewGuid(),

> CategoryID = Guid.Parse(\"7C318165-4FD6-4FE2-A85C-069B08AE611F\"),

Content = \"the context if the blog\",

Title = \"Taliban won the war\",

Summary = \"Taliban kick American\"

});

modelBuilder.Entity\<Blog\>().HasData(new Blog(){

BlogID = Guid.NewGuid(),

> CategoryID = Guid.Parse(\"7C318165-4FD6-4FE2-A85C-069B08AE611F\"),

Title = \"Title1\",

Content = \"World news\",

Summary = \"World news\",

});

}

**3.5 Creating migration through NUGET packet manager terminal**

At the very first time, you defined the initial domain classes. At this
point, there is no database for your application which can store the
data from your domain classes. So, firstly, you need to create a
migration.

Before issue any command for managing migration, check if you have
install Microsoft.EntityFrameworkCore.Tools. After that, open NuGet
packet manager terminal and type command line:

PM\> add-migration NameOfMigration

Then the migration folder will be automatically generated for you.

( Note: Ensure there is no project which contains error, so the build
process from add-migration is successful.)

Use the following command to create or update the database schema.

PM\> Update-Database
