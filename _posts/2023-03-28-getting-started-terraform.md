---
layout: post
title:  "Getting Started with Terraform: Variable Declaration, Local Values, Variable Usage, and Defining Outputs"
date:   2023-03-28
categories: terraform infrastructure-as-code variable declaration local values variable usage defining outputs aws hcl hashicorp cloud provisioning resource management automation scalability flexibility efficiency
---

Terraform is an open-source infrastructure as code tool that enables users to manage cloud resources efficiently. It is widely used by DevOps engineers to create, manage, and update their infrastructure as code. In a previous blog post, we discussed how to deploy a Kubernetes cluster and deploy a Helm Chart to it on OpenStack using Terraform. In this post, we will provide some quick and practical knowledge for working with Terraform.
<!--more-->
## Terraform Installation on Linux

Installation of Terraform is a straightforward process. Once you have installed the necessary dependencies, you can install Terraform using the following commands:

```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
touch ~/.bashrc
terraform -install-autocomplete
```

## Most Important Terraform Commands

After installation, we can proceed with the most important Terraform commands:

* `terraform init`: initializes a new or existing Terraform working directory by downloading provider modules and other dependencies.
* `terraform refresh`: updates the state file to reflect any changes made outside of Terraform.
* `terraform plan`: generates an execution plan that describes the changes that will be made to the infrastructure.
* `terraform apply -target=resource`: applies the changes to the infrastructure specified in the Terraform configuration files.
* `terraform destroy -target=resource`: removes the infrastructure resources that were created by Terraform.
* `terraform graph`: generates a visual representation of the Terraform resources and their dependencies.
* `terraform state list`: lists all resources in the current state file.
* `terraform show`: shows the current state or a specific resource in the state file.
* `terraform output`: displays the output values of a Terraform configuration.
* `terraform fmt`: formats the Terraform configuration files in a consistent style.
* `terraform validate`: checks the syntax and validates the Terraform configuration files.

## Defining Variables

Variables are essential in Terraform configuration files as they enable users to define values that can be used throughout the configuration. There are different ways to declare variables in Terraform.

You can define them in a `variables.tf` file:

```hcl
variable "server_port" {
  description = "The port the server will use for HTTP requests"
  type        = number
  default     = 8080
}
```

You can also override the default values by passing in values during plan or apply:

```bash
terraform plan -var "server_port=8080"
```

Alternatively, you can set the value of a variable using environment variables:

```bash
export TF_VAR_server_port=8080
```

## Defining Local Values

Terraform allows you to define local values that can be used within your code. Local values are useful for defining reusable values that are used across multiple resources or modules. Local values are defined using the locals block, as shown below:

```hcl
locals {
  http_port    = 80
  any_port     = 0
  any_protocol = "-1"
  tcp_protocol = "tcp"
  all_ips      = ["0.0.0.0/0"]
}
```

These values can then be used within your resources or modules by prefixing the name with "local.", for example:

```hcl
resource "aws_security_group_rule" "allow_http" {
  type        = "ingress"
  from_port   = local.http_port
  to_port     = local.http_port
  protocol    = local.tcp_protocol
  cidr_blocks = local.all_ips
}
```

### Variable Usage

You can use variables in your Terraform code by prefixing the variable name with "var.", as shown below:

```hcl
resource "aws_instance" "example" {
  ami           = var.ami_id
  instance_type = var.instance_type
  key_name      = var.key_name
  subnet_id     = var.subnet_id
}
```

This allows you to easily reuse the same code with different variables, making it easy to create multiple instances with different properties.

## Defining Outputs

Terraform allows you to define outputs that will be displayed at the end of a plan or apply. This is useful for providing information about your infrastructure that might be useful to other teams or for debugging purposes. Outputs are defined using the output block, as shown below:

```hcl
output "instance_ip" {
  value = aws_instance.example.public_ip
}
```

In the above example, the public IP address of the AWS instance will be displayed at the end of a plan or apply. You can also specify other configuration options for the output, such as description and sensitive, to customize the output.

## Conclusion

In this blog post, we covered some of the basics of using Terraform, including variable declaration, local values, variable usage, and defining outputs. Terraform is a powerful tool that allows you to define your infrastructure as code, making it easy to create, update, and destroy your infrastructure in a safe and repeatable manner. If you want to learn more about Terraform, we recommend reading Terraform: Up and Running, 3rd Edition by Yevgeniy Brikman.
