# Local Deployment using Infrastructure as Code

This guide describes how to deploy a two-party Carbyne Stack Virtual Cloud
onto a local Kubernetes cluster using CDKTF.

> **DISCLAIMER**: Carbyne Stack Infrastructure as Code (IaC) is in
> *proof-of-concept* stage. The software is not ready for production
> use. It has neither been developed nor tested for a specific use case.

## Prerequisites

Before you begin, ensure you have met the following requirements:

- [Node.js](https://nodejs.org/en/download) v18.17.1
- [Terraform CLI](https://developer.hashicorp.com/terraform/downloads) v1.5.5
- [CDKTF CLI](https://developer.hashicorp.com/terraform/tutorials/cdktf/cdktf-install)
  v0.18.0
- [Docker](https://docs.docker.com/engine/install/ubuntu/) v23.0.1
- [Kind](https://kind.sigs.k8s.io/) v0.17.0
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) v1.26.1
- [Helm](https://helm.sh/docs/intro/install/) v3.11.1

## Setup

1. Clone the carbynestack/carbynestack repository

    ```shell
    git clone git@github.com:carbynestack/carbynestack.git
    ```

1. Change into the `carbynestack` directory

    ```shell
    cd carbynestack
    ```

1. Checkout the tag `sdk-v0.4.0`

    ```shell
    git checkout tags/sdk-v0.4.0
    ```

1. Change directory to the `deployments` folder

    ```shell
    cd deployments
    ```

1. Install npm dependencies:

    ```shell
    npm install
    ```

1. Generate CDKTF provider bindings and import modules (located in the `.gen` folder):

    ```shell
    cdktf get
    ```

## Deploy

In the `./deployments` folder:

1. Deploy the stack using

```shell
cdktf deploy local-kind
```

## Destroy and Clean Up

If you no longer need the stack or want to tear it down to apply changes to the
infrastructure as code, run the following command:

```bash
cdktf destroy
```

Alternatively, you can use:

```bash
kind delete clusters cs-1 cs-2
```

And delete the ckdtf state files (like `terraform.local-kind.tfstate`)
