làm sao để thêm parameter và input trong 1 terraform?

cách thức tổ chức thư mục của một terraform project để dể maintain

cách import 1 file env từ bên ngoài

Use Terraform’s file and fileexists functions to interact with the local filesystem.

```
variable "config_file_path" {
	description = "Path to the configuration file"
	type = string
	default = "config.txt"
}

locals {
	file_content = fileexists(var.config_file_path) ? file(var.config_file_path) : "Default content"
}
output "file_content" {
value = local.file_content
}
```
Excessive reliance on local files can make your Terraform configurations less portable and
more challenging to manage in team environments.

```
data "aws_subnets" "main" {
	filter {
		name = "vpc-id"
		values = ["vpc-9ba9b5c6db85a9918"]
	}
}
```

can we organize terraform file as module
