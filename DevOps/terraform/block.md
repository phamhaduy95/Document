
#### data source 
Terraform doesn’t just provide the ability to create new resources. You can use another class of object called a data source to pull in information from outside of Terraform.
they instead use their arguments to search and filter data from their
provider and make that data available to the rest of the program.

create_before_destroy: true
deletion policy

```
# aws_vpc is resource type while "default" is data name
# create new data name "default" 
data "aws_vpc" "default" {
	default = true
}
```

resource base structure
```
resource "resource_type" "unique_resource_name" {
	string_argument = "value"
	integer_argument = 134
	boolean_argument = true
	
	object_argument = {
		"key" = "value"
	}
	
	lifecyle {
		create_before_destroy = true
	}
}
```

```
resource "aws_instance" "hello_world" {
	ami = data.aws_ami.ubuntu.id
	subnet_id = data.aws_subnet_ids.main.ids[0]
	instance_type = var.instance_type
}
```