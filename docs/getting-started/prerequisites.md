# Setting up the prerequisites

This guideline includes the steps to install the prerequisites with specified versions for the Carbyne Stack. All installation procedures are done with the Terminal application of Ubuntu. **Please open the Terminal application before you proceed.**

## Prerequisites

This part of the tutorial is developed and tested with Ubuntu 20.04. Please refer to this [link](https://ubuntu.com/tutorials/install-ubuntu-desktop) for Ubuntu installation steps.

## Platform Setup Prerequisites

Software to be installed:

- [go](https://go.dev/doc/install)
- [Docker Engine](https://docs.docker.com/engine/install/ubuntu/) v20.10.6
- [Kind](https://kind.sigs.k8s.io/) v0.11.0
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) v1.21.1
- [Helm](https://helm.sh/docs/intro/install/) v3.7.1

### go

The go language is a prerequisite for the Docker Engine. In this guideline go 1.18 will be installed.

1. Download go for Linux.

    ```shell
    wget https://go.dev/dl/go1.18.linux-amd64.tar.gz
    ```

2. Extract to /usr/local. You may need to insert the password for sudo permissions.

    ```shell
    sudo tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
    ```

3. Check if there is a “go” folder in "/usr/local".

    ```shell
    ls /usr/local
    ```
 
4. Update PATH for go.

    ```shell
    echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
    source ~/.bashrc
    ```

5. Verify if PATH is updated. Now go 1.18 is successfully installed.

    ```shell
    go version
    ```

### Docker Engine

1. Update repository index.

    ```shell
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg lsb-release
    ```

2. Add Docker’s official GPG key.

    ```shell
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    ```

3. Set up the stable repository.

    ```shell
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```

4. Install Docker Engine.

    ```shell
    sudo apt-get update
    sudo apt-get install docker-ce=5:20.10.6~3-0~ubuntu-focal docker-ce-cli=5:20.10.6~3-0~ubuntu-focal containerd.io
    ```

5. Manage Docker as a non-root user.

    ```shell
    sudo groupadd docker
    sudo usermod -aG docker $USER
    newgrp docker
    ```

6. Verify the installation.

    ```shell
    docker run hello-world
    ```

### Kind

1. Install Kind 0.11.0.

    ```shell
    go install sigs.k8s.io/kind@v0.11.0
    ```

2. Add the go path to PATH. You may have a different go path. Please check “GOPATH” with “go env” and replace “~/go” in the below command with your GOPATH.

    ```shell
    echo 'export PATH=$PATH:~/go/bin' >> ~/.bashrc 
    source ~/.bashrc
    ```

3. Verify Kind installation.

    ```shell
    kind version
    ```

### kubectl

1. Download kubectl 1.21.1.

    ```shell
    curl -LO https://dl.k8s.io/release/v1.21.1/bin/linux/amd64/kubectl
    ```

2. Install kubectl.

    ```shell
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```

3. Verify the installation.

    ```shell
    kubectl version --client
    ```

### Helm

1. Download Helm.

    ```shell
    wget https://get.helm.sh/helm-v3.7.1-linux-amd64.tar.gz
    ```

2. Unpack.

    ```shell
    tar -zxvf helm-v3.7.1-linux-amd64.tar.gz
    ```

3. Move to PATH.

    ```shell
    sudo mv linux-amd64/helm /usr/local/bin/helm
    ```

4. Verify Installation.

    ```shell
    helm version
    ```

## Stack Deployment Prerequisites

Software to be installed:

- [Helmfile](https://github.com/roboll/helmfile) v0.142.0
- [Helm Diff Plugin](https://github.com/databus23/helm-diff) v3.1.3
- [OpenJDK](https://openjdk.java.net/install/) 8

### Helmfile

1. Download Helmfile.

    ```shell
    wget https://github.com/roboll/helmfile/releases/download/v0.142.0/helmfile_linux_amd64
    ```

2. Give execution permission.

    ```shell
    chmod +x helmfile_linux_amd64
    ```

3. Move to PATH.

    ```shell
    sudo mv helmfile_linux_amd64 /usr/local/bin/helmfile
    ```

4. Verify Installation.

    ```shell
    helmfile -v
    ```

### Helm Diff Plugin

1. Download Helm Diff Plugin.

    ```shell
    wget https://github.com/databus23/helm-diff/releases/download/v3.1.3/helm-diff-linux.tgz
    ```

2. Unpack.

    ```shell
    tar -zxvf helm-diff-linux.tgz
    ```

3. Put the unpacked contents into the helm plugins folder. You may have a different folder. Please check “HELM_PLUGINS” with “helm env” command. Please create the folders if they do not exist. Note that the "diff" folder must not exist in the helm plugins folder before executing the command below.

    ```shell
    mv diff ~/.local/share/helm/plugins/diff
    ```

4. Verify the installation.

    ```shell
    helm plugin list
    ```

### OpenJDK

1. Install with command.

    ```shell
    sudo apt-get install openjdk-8-jdk
    ```

2. Verify installation.

    ```shell
    java -version
    ```
