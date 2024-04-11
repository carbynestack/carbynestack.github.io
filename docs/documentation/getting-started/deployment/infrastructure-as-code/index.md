# Infrastructure as Code

!!! warning
Carbyne Stack Infrastructure as Code (IaC) is still in *proof-of-concept*
stage. Reach out in case you encounter problems.

Carbyne Stack has adopted Infrastructure as Code (IaC) as a core principle.
IaC is the process of managing and provisioning infrastructure through code
instead of manually deploying resources via kubectl, helm, etc. This allows
for the infrastructure to be versioned, tested, and deployed in a repeatable
manner.

## Terraform

Carbyne Stack uses [Terraform by Hashicorp](https://www.terraform.io/) to
provision infrastructure and deploy Carbyne Stack on top of it. Terraform
has a large community and is widely used in the industry, with support for
most major cloud providers through a large set
of [providers](https://registry.terraform.io/browse/providers?product_intent=terraform).

## Cloud Development Kit for Terraform (CDKTF)

Terraform uses a custom syntax for their configuration language called
*HashiCorp Configuration Language* (HCL). While this is great for simple use
cases, it can become difficult to manage as the complexity of the
infrastructure grows. To address this, Carbyne Stack uses
[CDKTF](https://learn.hashicorp.com/tutorials/terraform/cdktf), which
allows you to use a common programming language, in Carbyne Stack's
case, Typescript, to define the infrastructure. This allows you to
leverage the full power of a programming language to define your
infrastructure, including the ability to use loops, conditionals,
functions, and abstractions.

### Stacks

In CDKTF,
[stacks](https://developer.hashicorp.com/terraform/cdktf/concepts/stacks)
represent a collection of resources that are deployed together. Carbyne Stack
uses a separate stack for each deployment target with each stack being
responsible for provisioning the infrastructure and deploying Carbyne Stack on
top of it.

As of today, the following deployment targets are supported:

- [Local deployment](./local) to [kind](https://kind.sigs.k8s.io/) clusters
- [Azure](./azure) to
  [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/products/kubernetes-service)
  clusters
- [Azure Private](./azure-private) to
  [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/products/kubernetes-service)
  private clusters

### Constructs

In CDKTF,
[constructs](https://developer.hashicorp.com/terraform/cdktf/concepts/constructs)
are the building blocks of the Carbyne Stack infrastructure. Constructs are used
to define resources, modules, and data sources. Carbyne Stack uses constructs
to define the resources that make up the infrastructure, including the
Kubernetes cluster, networking, and storage. Constructs are also used to
define the Carbyne Stack Helm chart, which is used to deploy Carbyne Stack on
top of the infrastructure.

## Prerequisites

Before you proceed, make sure you meet the following requirements:

- [Node.js](https://nodejs.org/en/download) v18.17.1
- [Terraform CLI](https://developer.hashicorp.com/terraform/downloads) v1.5.5
- [CDKTF CLI](https://developer.hashicorp.com/terraform/tutorials/cdktf/cdktf-install)
  v0.18.0
- [Docker](https://docs.docker.com/engine/install/ubuntu/) v23.0.1
- [Kind](https://kind.sigs.k8s.io/) v0.17.0
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) v1.26.1
- [Helm](https://helm.sh/docs/intro/install/) v3.11.1
