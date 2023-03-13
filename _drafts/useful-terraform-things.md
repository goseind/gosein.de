---
layout: post
title:  "Getting started with Terraform, a quick guide with all you need to know to get going"
date:   2023-03-13
categories: terraform iac infrastructure as code
---

In my last blog post [Working with Terraform: Building a K8s cluster and deploying a Helm Chart to it on OpenStack](https://gosein.de/working-with-terraform.html) I described a specific use case when using terraform to deploy a Kubernetes cluster. With this post, I want to provide some quick and practical knowledge for working with Terraform.

First up, installing `terraform` with:

```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
touch ~/.bashrc
terraform -install-autocomplete
```

The most important terraform commands:

```bash
terraform init
terraform refresh
terraform plan
terraform apply -target=resource
terraform destroy -target=resource
terraform graph
terraform state list
terraform show
terraform output
terraform fmt
terraform validate
```

Variables declaration:

```hcl
variable "server_port" {
  description = "The port the server will use for HTTP requests"
  type        = number
  default     = 8080
}
```

or:

```bash
terraform plan -var "server_port=8080"
TF_VAR_<name>
```

Defining *local values* `local.<NAME>`:

```hcl
locals {
  http_port    = 80
  any_port     = 0
  any_protocol = "-1"
  tcp_protocol = "tcp"
  all_ips      = ["0.0.0.0/0"]
}
```

Using variables:

```bash
var.<VARIABLE_NAME>
```

Defining output from a resource to show at the end of `plan`/`apply`:

```hcl
output "<NAME>" {
  value = <VALUE>
  [CONFIG ...]
}
```

Recommended reading: Terraform: Up and Running, 3rd Edition by Yevgeniy Brikman
