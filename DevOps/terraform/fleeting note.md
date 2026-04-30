```terraform
resource "<PROVIDER>_<TYPE>" "<NAME>" {
	[CONFIG ...]
}
```

``` terraform
resource "aws_instance" "example" {
	ami = "ami-0fb653ca2d3203ac1"
	instance_type = "t2.micro"
}
```

`terraform init

`terraform plan`: let you see what `terraform` would do before actual applying change

`terrafrom apply`



The `<<-EOF and EOF` are Terraform’s heredoc syntax, which allows you to create
multi-line strings without having to insert \n characters all over the place.

 The user_data_replace_on_change parameter is set to true so that when you
change the user_data parameter and run apply, Terraform will terminate the
original instance and launch a totally new one


`aws_security_group.instance.id`


zero-downtime deployment


Terraform keeps track of what resources you created, cleanup is simple. All you need to do is run the destroy command

Every time you run Terraform, it records information about what infrastructure
it created in a Terraform state file.

It’s too easy to forget to pull down the latest changes from version control
before running Terraform or to push your latest changes to version control after
running Terraform. It’s just a matter of time before someone on your team runs
Terraform with out-of-date state files and, as a result, accidentally rolls back or
duplicates previous deployments.



You can use Terraform’s native
constructs, such as variables, data sources, and remote states, to share information
between different configuration parts.

terraform secret



input variables


#### terraform components

Blocks are the primary language construct of Terraform




Immutability through `IaC` allows version control to manage infrastructure configuration as a source of truth and facilitate future reproduction.


Bundling the required resources together helps your teammate who might
not know much about networking. The composite pattern improves the principle of composability because it groups and organizes common resources that you must deploy as one unit.

The composite pattern works well for infrastructure because infrastructure
resources have a hierarchy. A module following this pattern reflects relationships among resources and facilitates their management



 An infrastructure dependency expresses a relationship in which
an infrastructure resource depends on the existence and attributes of
another resource.


Make sure the folder doesn’t contain any existing configuration code, because Terraform concatenates all .tf files together.