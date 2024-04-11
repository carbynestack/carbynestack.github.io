# Local Deployment using Infrastructure as Code

This guide describes how to deploy a two-party Carbyne Stack Virtual Cloud
onto an Azure private Kubernetes cluster using Infrastructure as Code (IaC).

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

6. Generate CDKTF provider bindings and import modules (located in the `.gen` folder):

    ```shell
    cdktf get
    ```

## Azure

CDKTF uses the Azure CLI under the hood to authenticate and interact with Azure.

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

## Deploy the Jump Host

In the `./deployments` folder:

1. Deploy the Jump Host using a provided password that will be used to
   access the Jump Host:

    ```shell
    cdktf deploy azure-jump --var='admin_password=<password>'
    ```

2. Once the Jump Host is deployed you will see the IP address of the
   Jump Host in the CDKTF output.

    ```shell
    ssh caliper@<jump_host_ip>
    ```

## Deploy the Private AKS Cluster with Carbyne Stack

On the jump host, you need to install the following dependencies before deploying:

1. Install OS dependencies:

    ```shell
    sudo apt update && sudo apt install -y  make build-essential gnupg software-properties-common apt-transport-https ca-certificates curl
    ```

2. Install npm via nvm since it deals with permission issues for global packages

    ```shell
    curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
    export NVM_DIR="$HOME/.nvm"
    source ~/.bashrc
    nvm install --lts
    ```

3. Install kubectl:

    ```shell
    snap install kubectl --classic
    ```

4. Install Terraform:

    ```shell
    wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
    echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
    sudo apt update && sudo apt install terraform
    ```

5. Install Azure CLI:

    ```shell
    curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
    ```

6. Install CDKTF and TypeScript:

    ```shell
    npm install --global cdktf-cli@0.17.1 typescript
    ```

7. Clone the Carbyne Stack repository and build:

    ```shell
    git clone -b iac-azure https://github.com/carbynestack/carbynestack.git
    cd carbynestack/deployments
    npm install
    cdktf get && npm run build
    ```

8. Set Azure credentials:

    ```shell
    export ARM_CLIENT_ID=<appId>
    export ARM_CLIENT_SECRET=<password>
    export ARM_TENANT_ID=<tenant>
    export ARM_SUBSCRIPTION_ID=<subscription id>
    ```

9. Deploy the Carbyne Stack on the private AKS cluster:

    ```shell
    JUMP_HOST_RESOURCE_GROUP=rg-cs-jump JUMP_HOST_VIRTUAL_NETWORK_NAME=vn-cs-jump cdktf deploy azure-private-cluster
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
