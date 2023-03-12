---
layout: post
title:  "Working with Terraform: Some useful things"
date:   2023-XX-XX
categories: terraform iac infrastructure as code
---

Terraform install on Linux:

```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
touch ~/.bashrc
terraform -install-autocomplete
```

Terraform basic commands:

```bash
terraform init
terraform refresh
terraform plan
terraform apply -target=resource
terraform graph
terraform state list
terraform show
terraform output
terraform destroy
```

Terraform format and validation e. g. in CI/CD:

```bash
terraform fmt
terraform validate
```

Terraform variables declaration:

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

Terraform variable usage:

```bash
var.<VARIABLE_NAME>
```

Defining output form a ressource to show at the end of plan/apply:

```hcl
output "<NAME>" {
  value = <VALUE>
  [CONFIG ...]
}
```
