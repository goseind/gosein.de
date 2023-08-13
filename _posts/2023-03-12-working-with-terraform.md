---
layout: post
title:  "Working with Terraform: Building a K8s cluster and deploying a Helm Chart to it on OpenStack"
date:   2023-03-12
categories: terraform openstack kubernetes iac infrastructure as code helm chart
---

Infrastructure as Code (IaC) has become a popular approach to managing IT infrastructure. This approach treats infrastructure components as code, allowing automated deployment and management of infrastructure resources across different platforms, with the advantage of having all this information stored and tracked in a repository.
In this blog post, we want to explore [Terraform by HashiCorp](https://www.terraform.io/) - which is a popular tool for this purpose - by building a Kubernetes cluster on OpenStack and installing a Helm Chart on it.
<!--more-->
Terraform has its own language to define resources, called [HCL](https://github.com/hashicorp/hcl/blob/main/hclsyntax/spec.md). While all the resources could be written in one file, it makes sense to divide them into several `.tf` files. Terraform reads all these files within the folder/subfolder, when initialized. 
We'll start with a `providers.tf` file, where we define the providers we'll use. Providers allow you to interact with the corresponding API to manage resources.

For our goal we need three providers: [OpenStack](https://registry.terraform.io/providers/terraform-provider-openstack/openstack/1.50.0), [Kubernetes](https://registry.terraform.io/providers/hashicorp/kubernetes/latest), and [Helm](https://registry.terraform.io/providers/hashicorp/helm/latest):

```hcl
terraform {
  required_providers {
    openstack = {
      source  = "terraform-provider-openstack/openstack"
      version = "1.49.0"
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "2.16.0"
    }
    helm = {
      source  = "hashicorp/helm"
      version = "2.8.0"
    }
  }
}
```

Further, terraform needs a backend to store the configuration ([state](https://developer.hashicorp.com/terraform/language/state)), the default is local, but to work in a team you want to store it in a secure remote location. In our case we're first going to use the local backend until we've set up our K8s cluster and then switch to that one:

```hcl
terraform {
  backend "local" {
    path = "relative/path/to/terraform.tfstate"
  }
}
```

Each provider need a configuration. OpenStack is our main entry point and we're going to set the [required arguments](https://registry.terraform.io/providers/terraform-provider-openstack/openstack/latest/docs#configuration-reference) as environment variables and thus leave the brackets empty:

```hcl
provider "openstack" {
}
```

Now we can create our first resource, an OpenStack Kubernetes Cluster. On my platform (CERN Openstack), this requires us to create a keypair for the compute service SSH first:

```hcl
data "openstack_containerinfra_clustertemplate_v1" "cluster_template" {
  name = var.cluster-template-name
}

resource "openstack_compute_keypair_v2" "openstack_cluster_keypair" {
  name = var.cluster-keypair-name
}

resource "openstack_containerinfra_cluster_v1" "openstack_cluster" {
  name                = var.cluster-name
  cluster_template_id = data.openstack_containerinfra_clustertemplate_v1.cluster_template.id
  master_count        = 2
  node_count          = 4
  keypair             = var.cluster-keypair-name
  merge_labels        = true
  flavor              = "m2.xlarge"
  master_flavor       = "m2.large"
  provisioner "local-exec" {
    command = "sh relative/path/to/post_cluster_setup.sh"
    environment = {
      cluster = var.cluster-name
    }
  }
}
```

As you've noticed, we can use variables to set the values and data sources from the same provider, to get existing information like the existing cluster templates ID. It might be useful to define variables in a dedicated file outside the resources, like so:

```hcl
variable "cluster-template-name" {
  description = "The cluster template"
  type        = string
  default     = "kubernetes-1.22.9-1-multi"
}

variable "cluster-name" {
  description = "The openstack cluster name"
  type        = string
  default     = "testcluster"
}

variable "cluster-keypair-name" {
  description = "The cluster keypair name"
  type        = string
  default     = "testcluster-keypair"
}
```

The [`provisioner "local-exec" {}`](https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec) can be used to invoke a local executable after the resource is created. This is useful if you need to do configuration that is not otherwise possible with terraform resources.

Now we're ready to run the code by executing the following commands one after the other:

1. `terraform init` to load the configuration and providers
2. `terraform plan` to verify the changes we're about to make
3. `terraform apply` to deploy the changes

Now that the cluster is deployed, we can use the existing resource to set up the configuration of the K8s and Helm provider by pulling the information (kube config) from it:

```hcl
provider "kubernetes" {
  host                   = openstack_containerinfra_cluster_v1.openstack_cluster.kubeconfig.host
  cluster_ca_certificate = openstack_containerinfra_cluster_v1.openstack_cluster.kubeconfig.cluster_ca_certificate
  client_certificate     = openstack_containerinfra_cluster_v1.openstack_cluster.kubeconfig.client_certificate
  client_key             = openstack_containerinfra_cluster_v1.openstack_cluster.kubeconfig.client_key
}

provider "helm" {
  kubernetes {
    host                   = openstack_containerinfra_cluster_v1.openstack_cluster.kubeconfig.host
    cluster_ca_certificate = openstack_containerinfra_cluster_v1.openstack_cluster.kubeconfig.cluster_ca_certificate
    client_certificate     = openstack_containerinfra_cluster_v1.openstack_cluster.kubeconfig.client_certificate
    client_key             = openstack_containerinfra_cluster_v1.openstack_cluster.kubeconfig.client_key
  }
}
```

Further, we can migrate the state to the Kubernetes cluster:

```hcl
terraform {
  backend "kubernetes" {
    secret_suffix = "state"
    config_path   = "~/.kube/config" # Change to your local config path if necessary (variables cannot be used inside here)
    namespace     = "default"
  }
}
```

This backend uses the kube config to authenticate to the cluster and create/update a secret.

Next, we create a namespace with the Kubernetes provider:

```hcl
resource "kubernetes_namespace_v1" "ns_shared_services" {
  metadata {
    name = var.ns-shared-services
  }
}
```

Again using a variable:

```hcl
variable "ns-shared-services" {
  description = "The name of the namespace for shared services"
  type        = string
  default     = "shared-services"
}
```

Finally, we can install a Helm Chart (in this case Sealed Secrets):

```hcl
resource "helm_release" "sealed-secrets-chart" {
  name       = "sealed-secrets"
  repository = "https://bitnami-labs.github.io/sealed-secrets"
  chart      = "sealed-secrets"
  version    = "2.7.1"
  namespace  = var.ns-shared-services
}
```

Different values can be set either by specifying a YAML file or setting them explicitly (again data sources can be used here to obtain secrets for example):

```hcl
  values = [
    "${file("values.yaml")}"
  ]

  set {
    name  = "updateStatus"
    value = "true"
  }
```

Thats it!

The entire combined code is summarized in this gist:

{% gist 8c688fb24d9bd47e36cd823e6aa90e61 %}
