---
layout: post
title:  "Streamlining Kubernetes Deployments: Using Terraform to Maintain Flux"
date:   2023-03-21
categories: terraform iac infrastructure-as-code flux gitops deployments kubernetes k8s
---

As more and more organizations adopt Kubernetes for container orchestration, it has become increasingly important to have a robust and reliable tool for managing Kubernetes deployments. Flux, an open-source tool for GitOps continuous delivery, has emerged as one of the most popular tools for managing Kubernetes deployments. Terraform, another popular tool for infrastructure management, can be used to manage the installation and configuration of Flux. In this blog post, we will explore the benefits of using Terraform to maintain Flux.
<!--more-->
The [tf-flux](https://github.com/goseind/tf-flux) repository is a great resource for anyone looking to get started with using Terraform to manage Flux installations. The repository showcases the usage of the [terraform fluxcd provider](https://registry.terraform.io/providers/fluxcd/flux/latest), which allows users to create, manage, and delete Flux installations from Terraform.

To get started, simply clone the repository and navigate to the `tf` directory. Run the command `terraform init` to initialize the Terraform environment. Then, run the command `terraform apply` and enter your GitHub organizations name, repository, and token. Once this is done, any new files in the `flux/cluster` directory will be synced with Flux, while the Flux installation itself is maintained by Terraform.

Looking at the [main.tf](https://github.com/goseind/tf-flux/blob/main/tf/main.tf) file, you'll see that we're using two providers, `fluxcd/flux` and `integrations/github`:

```hcl
terraform {
  required_version = ">=1.1.5"
  required_providers {
    flux = {
      source  = "fluxcd/flux"
      version = ">=0.25.2"
    }
    github = {
      source  = "integrations/github"
      version = ">=5.18.3"
    }
  }
}

provider "github" {
  owner = var.github_org
  token = var.github_token
}

resource "tls_private_key" "flux" {
  algorithm   = "ECDSA"
  ecdsa_curve = "P256"
}

resource "github_repository_deploy_key" "this" {
  title      = "Flux"
  repository = var.github_repository
  key        = tls_private_key.flux.public_key_openssh
  read_only  = "false"
}

provider "flux" {
  kubernetes = {
    config_path = "~/.kube/config"
  }
  git = {
    url = "ssh://git@github.com/${var.github_org}/${var.github_repository}.git"
    ssh = {
      username    = "git"
      private_key = tls_private_key.flux.private_key_pem
    }
  }
}
```

The latter is needed to obtain a deployment key to update the repository:

```hcl
resource "tls_private_key" "flux" {
  algorithm   = "ECDSA"
  ecdsa_curve = "P256"
}

resource "github_repository_deploy_key" "this" {
  title      = "Flux"
  repository = var.github_repository
  key        = tls_private_key.flux.public_key_openssh
  read_only  = "false"
}
```

In the final step, we're bootstrapping the repo to the cluster and setting a `path` for Flux to store system files. This path can be the same as `target_path` which is the folder Flux syncs with:

```hcl
resource "flux_bootstrap_git" "this" {
  depends_on = [github_repository_deploy_key.this]
  path = "flux"
}

data "flux_install" "main" {
  target_path    = "flux/cluster"
  network_policy = false
  version        = "latest"
}

data "flux_sync" "main" {
  target_path = "flux/cluster"
  url         = "https://github.com/${var.github_org}/${var.github_repository}"
  branch      = "main"
}
```

One of the biggest benefits of using Terraform to maintain Flux is the ability to manage the entire infrastructure as code. While Flux sits on top of an existing Kubernetes cluster and can maintain manifests in it, with Terraform however you can maintain the cluster and its surrounding resources as well and thus combine the two seamlessly.  
With Terraform, it is easy to version control, test, and automate infrastructure changes. This makes it easier to maintain a consistent and reliable infrastructure over time.

Another benefit of using Terraform to maintain Flux is that it simplifies the installation process. Instead of manually installing and configuring Flux, Terraform can automate the entire process, which saves time and reduces the risk of human error.

Finally, using Terraform to maintain Flux makes it easier to scale and manage multiple Flux installations. With Terraform, it is easy to create and manage multiple Flux installations across multiple environments, which simplifies the process of managing Kubernetes deployments at scale.

In conclusion, the tf-flux repository is a great resource for anyone looking to get started with using Terraform to maintain Flux installations. By using Terraform to maintain Flux, organizations can easily manage their infrastructure as code, simplify the installation process, and scale and manage multiple installations across multiple environments. If you are looking to streamline your Kubernetes deployment process, then using Terraform to maintain Flux is definitely worth exploring.
