# Infrastructure as Code

CarbyneStack has adopted Infrastructure as Code (IaC) as a core principle.
IaC is the process of managing and provisioning infrastructure through code
instead of manually deploying resources via kubectl, helm, etc. This allows
for the infrastructure to be versioned, tested, and deployed in a repeatable
manner.

## Terraform

CarbyneStack uses [Terraform by Hashicorp](https://www.terraform.io/) to
provision infrastructure and deploy CarbyneStack on top of it.  Terraform
has a large community and is widely used in the industry, with support for
most major cloud providers through a large set
of [providers](https://registry.terraform.io/browse/providers?product_intent=terraform).

## Cloud Development Kit for Terraform (CDKTF)

Terraform uses a custom syntax for their configuration language called HCL
(HashiCorp Configuration Language).  While this is great for simple use
cases, it can become difficult to manage as the complexity of the
infrastructure grows.  To address this, CarbyneStack uses
[CDKTF](https://learn.hashicorp.com/tutorials/terraform/cdktf), which
allows you to use a common programming language, in CarbyneStack's
case, Typescript, to define the infrastructure. This allows you to
leverage the full power of a programming language to define your
infrastructure, including the ability to use loops, conditionals,
functions, and abstractions.  

### Stacks

In CDKTF,
[stacks](https://developer.hashicorp.com/terraform/cdktf/concepts/stacks)
represent a collection of resources that are deployed together.
CarbyneStack uses a separate stack per cloud provider, including local,
with each stack being responsible for provisioning the infrastructure and
deploying the CarbyneStack on top of it.  

### Constructs

In CDKTF,
[constructs](https://developer.hashicorp.com/terraform/cdktf/concepts/constructs)
are the building blocks of the CarbyneStack infrastructure.Constructs are used
to define resources, modules, and data sources.  CarbyneStack uses constructs
to define the resources that make up the infrastructure, including the
Kubernetes cluster, networking, and storage.  Constructs are also used to
define the CarbyneStack Helm chart, which is used to deploy the
CarbyneStack on top of the infrastructure.
