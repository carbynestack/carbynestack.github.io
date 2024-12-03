# Local Deployment using Infrastructure as Code

This guide describes how to deploy a two-party Carbyne Stack Virtual Cloud
onto Azure.

## Setup

1. Clone the carbynestack/carbynestack repository

    ```shell
    git clone git@github.com:carbynestack/carbynestack.git
    ```

2. Change into the `carbynestack` directory

    ```shell
    cd carbynestack
    ```

3. Checkout the tag `sdk-v0.5.0`

    ```shell
    git checkout tags/sdk-v0.5.0
    ```

4. Change directory to the `deployments` folder

    ```shell
    cd deployments
    ```

5. Install npm dependencies:

    ```shell
    npm install
    ```

6. Generate CDKTF provider bindings and import modules
  (located in the `.gen` folder):

    ```shell
    cdktf get
    ```

## Azure

CDKTF uses the Azure CLI under the hood to authenticate and interact with
Azure.

1. Log in to Azure using the Azure CLI:

    ```shell
    az login
    ```

2. Determine your Azure subscription ID:

    ```shell
    az account list
    ```

    Find the subscription you want to use and save for later use.

3. Create a service principal:

    ```shell
    az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<your subscription id>"
    ```

    This command will ouput similar to the following example:

    ```shell
    {
      "appId": "<appId>",
      "displayName": "azure-cli-2022-01-01-00-00-00",
      "password": "<password>",
      "tenant": "<tenant>"
    }
    ```

    Save the `appId`, `password`, and `tenant` for later use.

4. Export the service principal credentials as environment variables:

    ```shell
    export ARM_CLIENT_ID=<appId>
    export ARM_CLIENT_SECRET=<password>
    export ARM_TENANT_ID=<tenant>
    export ARM_SUBSCRIPTION_ID=<subscription id>
    ```

## Deploy

In the `./deployments` folder:

1. Deploy the stack using

    ```shell
    cdktf deploy azure-cluster
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
