# Local Deployment using Infrastructure as Code

This guide describes how to deploy a two-party Carbyne Stack Virtual Cloud
onto local [kind](https://kind.sigs.k8s.io/) cluster using CDKTF.

## Setup

1. Clone the carbynestack/carbynestack repository

    ```shell
    git clone git@github.com:carbynestack/carbynestack.git
    ```

1. Change into the `carbynestack` directory

    ```shell
    cd carbynestack
    ```

1. Checkout the tag `sdk-v0.5.0`

    ```shell
    git checkout tags/sdk-v0.5.0
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

And delete the CDKTF state files (like `terraform.local-kind.tfstate`) manually.
