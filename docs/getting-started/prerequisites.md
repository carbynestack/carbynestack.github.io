# Setting up the prerequisites

This guide describes how to install the prerequisites required to deploy
Carbyne Stack.

!!! warning
    Carbyne Stack has been tested using the **exact versions** of the tools
    specified below. Deviating from this _battle tested_ configuration may
    create all kinds of issues.

## Prerequisites

This part of the tutorial is developed and tested with Ubuntu 20.04.
Please refer to this [link](https://ubuntu.com/tutorials/install-ubuntu-desktop)
for Ubuntu installation steps.

## Platform Setup Prerequisites

Software to be installed:

- [go](https://go.dev/doc/install) 1.18
- [Docker Engine](https://docs.docker.com/engine/install/ubuntu/) v20.10.6
- [Kind](https://kind.sigs.k8s.io/) v0.11.0
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) v1.21.1
- [Helm](https://helm.sh/docs/intro/install/) v3.7.1

Please set the following software version strings before you continue with our
installation instructions.

```shell
export go_ver=1.18
export dock_ver=5:20.10.6~3-0~ubuntu-focal
export kind_ver=0.11.0
export kub_ver=1.21.1
export helm_ver=3.7.1
```

### go

The go language is a prerequisite for the Kind package.
In this guideline go 1.18 will be installed.
Detailed installation instructions for go can be found [here](https://go.dev/doc/install).
Alternatively, you can install go by following the instructions below.

1. Download go language for Linux.

    ```shell
    wget https://go.dev/dl/go$go_ver.linux-amd64.tar.gz
    ```

2. Extract to `/usr/local`. You may need to insert the password for sudo permissions.

    ```shell
    sudo tar -C /usr/local -xzf go$go_ver.linux-amd64.tar.gz
    ```

3. Check if there is a `go` folder in `/usr/local`.

    ```shell
    ls /usr/local
    ```

4. Update `PATH` for go with the commands below.

    ```shell
    echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
    source ~/.bashrc
    ```

5. Verify if `PATH` is updated. Now go 1.18 is successfully installed.

    ```shell
    go version
    ```

### Docker Engine

Detailed installation instructions for Docker Engine can be found
[here](https://docs.docker.com/engine/install/ubuntu/).
Alternatively, you can install Docker Engine by following the instructions below.

1. Update repository index and install dependencies.

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

4. Install Docker Engine with the commands below.

    ```shell
    sudo apt-get update
    sudo apt-get install docker-ce=$dock_ver docker-ce-cli=$dock_ver containerd.io
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

Detailed installation instructions for Kind can be found [here](https://kind.sigs.k8s.io/).
Alternatively, you can install Kind by following the instructions below.

1. Install Kind 0.11.0.

    ```shell
    go install sigs.k8s.io/kind@v$kind_ver
    ```

2. Add the local go path to `PATH`.
   You may have a different go path.
   Please check `GOPATH` with `go env` and replace `~/go` in the below
   command with your `GOPATH`.

    ```shell
    echo 'export PATH=$PATH:~/go/bin' >> ~/.bashrc 
    source ~/.bashrc
    ```

3. Verify Kind installation.

    ```shell
    kind version
    ```

### kubectl

Detailed installation instructions for kubectl can be found
[here](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/).
Alternatively, you can install kubectl by following the instructions below.

1. Download kubectl 1.21.1.

    ```shell
    curl -LO https://dl.k8s.io/release/v$kub_ver/bin/linux/amd64/kubectl
    ```

2. Install kubectl with the command below.

    ```shell
    sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
    ```

3. Verify the installation.

    ```shell
    kubectl version --client
    ```

### Helm

Detailed installation instructions for the Helm package manager can be found
[here](https://helm.sh/docs/intro/install/).
Alternatively, you can install Helm by following the instructions below.

1. Download the Helm package manager.

    ```shell
    wget https://get.helm.sh/helm-v$helm_ver-linux-amd64.tar.gz
    ```

2. Unpack the downloaded compressed file.

    ```shell
    tar -zxvf helm-v$helm_ver-linux-amd64.tar.gz
    ```

3. Move the unpacked content (the `helm` file) to `PATH`.

    ```shell
    sudo mv linux-amd64/helm /usr/local/bin/helm
    ```

4. Verify your installation.

    ```shell
    helm version
    ```

## Stack Deployment Prerequisites

Software to be installed:

- [Helmfile](https://github.com/roboll/helmfile) v0.142.0
- [Helm Diff Plugin](https://github.com/databus23/helm-diff) v3.1.3
- [OpenJDK](https://openjdk.java.net/install/) 8

Please set the following software version strings before you continue with our
installation instructions.

```shell
export helmfile_ver=0.142.0
export helmdiff_ver=3.1.3
```

### Helmfile

Detailed installation instructions for the Helmfile package can be found
[here](https://github.com/roboll/helmfile).
Alternatively, you can install Helmfile by following the instructions below.

1. Download the Helmfile package.

    ```shell
    wget https://github.com/roboll/helmfile/releases/download/v$helmfile_ver/helmfile_linux_amd64
    ```

2. Give execution permission to Helmfile.

    ```shell
    chmod +x helmfile_linux_amd64
    ```

3. Move Helmfile to `PATH`.

    ```shell
    sudo mv helmfile_linux_amd64 /usr/local/bin/helmfile
    ```

4. Verify your installation.

    ```shell
    helmfile -v
    ```

### Helm Diff Plugin

Detailed installation instructions for the Helm Diff plugin can be found
[here](https://github.com/databus23/helm-diff).
Alternatively, you can install Helm Diff by following the instructions below.

1. Download Helm Diff plugin compressed file.

    ```shell
    wget https://github.com/databus23/helm-diff/releases/download/v$helmdiff_ver/helm-diff-linux.tgz
    ```

2. Unpack the compressed file.

    ```shell
    tar -zxvf helm-diff-linux.tgz
    ```

3. Put the unpacked contents into the helm plugins folder.
   You may have a different folder.
   Please check `HELM_PLUGINS` with `helm env` command.
   Please create the folders if they do not exist.
   Note that the `diff` folder must not exist in the helm plugins folder before
   executing the command below.

    ```shell
    mv diff ~/.local/share/helm/plugins/diff
    ```

4. Verify your installation.

    ```shell
    helm plugin list
    ```

### OpenJDK

Detailed installation instructions for OpenJDK can be found
[here](https://openjdk.java.net/install/).
Alternatively, you can install OpenJDK by following the instructions below.

1. Install with the command.

    ```shell
    sudo apt-get install openjdk-8-jdk
    ```

2. Verify your installation.

    ```shell
    java -version
    ```
