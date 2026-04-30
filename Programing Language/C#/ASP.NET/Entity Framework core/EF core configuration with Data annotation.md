1.  Introduction

This document will help you learn how to use Data annotations for entity
class configuration. Using data annotation has some advantages over
using fluent API:

- you write the configuration on the entity class directly not through
  OnModelCreating method in DbContext class.

- Writing configuration using data annotation is by far the most simple

However, it has some tradeoffs too:

- The fluent API offers more option and settings for fine-tunes
  configuration

- Using fluent API help separate the configuration task from entity
  class, which allows us to flexibly change or swap configuration on
  demand.

In my opinion, I recommend using fluent API over data annotation as
fluent API is more suitable in medium-scale or large-scale project thank
to its responsibility decoupling.

2.  Data annotation attribute

Data annotation consist of several attributes which can be added on top
of the entity class, data field to provide the configurations.

In this section, we will teach you how to apply data annotation in some
common and essential scenarios.

**2.1 Setting primary keys**

Adding Key attribute on top of one data field to promote them as the
primary key of the table.

1.  public class Order

2.  {

3.  \[Key\]

4.  public int OrderNumber { get; set; }

5.  public DateTime DateCreated { get; set; }

6.  public Customer Customer { get; set; }

7.  

8.  )

**2.2 Setting columns name and type**

You can preset the column name and type through column attribute.

1.  public class Book

2.  {

3.  public int BookId { get; set; }

4.  \[Column(\"Description\", TypeName = \"nvarchar(100)\")\]

5.  public string Title { get; set; }

6.  public Author Author { get; set; }

7.  }

**2.3 specifying foreign key**

public class Student

{

public int StudentID { get; set; }

public string StudentName { get; set; }

**\[ForeignKey(\"FK_Class\")\]**

**public int ClassID { get; set;}**

**public Class Class { get; set;}**

}

**2.4 set one column not nullable**

The Required attribute can be applied to one or more properties in an
entity class. EF will create a NOT NULL column in a database table for a
property on which the Required attribute is applied.

using System.ComponentModel.DataAnnotations;

public class Student

{

public int StudentID { get; set; }

\[Required\]

public string StudentName { get; set; }

}

**2.5 limit string length**

The StringLength attribute can be applied to the string properties of an
entity class. It specifies the maximum characters allowed for a string
property which in turn sets the size of a corresponding column (nvarchar
in SQL Server) in the database.

using System.ComponentModel.DataAnnotations;

public class Student

{

public int StudentID { get; set; }

\[StringLength(50)\]

public string StudentName { get; set; }

}
