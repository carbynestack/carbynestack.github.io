# Setting up the prerequisites with WSL2

This guide describes how to install the prerequisites required to deploy
Carbyne Stack via WSL2 in windows.

!!! warning
    Carbyne Stack has been tested using the **exact versions** of the tools
    specified below. Deviating from this _battle tested_ configuration may
    create all kinds of issues.

## Prerequisites

    WSL2 must be installed in your Windows machine.

    To install WSL2, run your Windows powershell and run this command

    ```shell
    wsl --install
    ```

Or you can refer to https://www.windowscentral.com/how-install-wsl2-windows-10 if more details are needed.

## Platform Setup Prerequisites

Software to be installed:

### install Ubuntu-20.04 inside WSL2 and connect to it
    ```shell
    wsl install Ubuntu-20.04\
    wsl -d Ubuntu-20.04
    ```

### Configure DNS Resolution
    1. prevent creating the resolve.conf on startup
    ```shell
    sudo nano /etc/wsl.conf
    ```
    And add this text to wsl.conf file

    ```shell
    [network]
    generateResolvConf = false
    ```

    then press CTRL+X to save your file

    2. Restart WSL via PowerShell
    ```shell
    wsl -d Ubuntuu-20.04 --shutdown
    ```
    then,
    ```shell
    wsl -d Ubuntu-20.04
    ```

    3. Add in your nameservers
    ```shell
    sudo nano /etc/resolv.conf
    ```

    And add this text to resolv.conf file
    ```shell
    nameserver 1.1.1.1
    nameserver 8.8.8.8
    ```

    then press CTRL+X to save your file

    4. Restart and reconnect like before
    ```shell
    wsl -d Ubuntuu-20.04 --shutdown
    ```
    then,
    ```shell
    wsl -d Ubuntu-20.04
    ```

### Install Docker
    1. Use legacy IPTables that the following docker install needs:

            ```shell
            sudo update-alternatives --set iptables /usr/sbin/iptables-legacy
            sudo update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
            ``` 

    2. Follow guide (Including Optional Step 2) @ https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-22-04

### Install Golang
    1. install go

            ```shell
            curl -OL https://go.dev/dl/go1.19.5.linux-amd64.tar.gz
            sudo tar -C /usr/local -xvf go1.19.5.linux-amd64.tar.gz
            ```

    2. setup PATH 

            ```shell
            sudo nano ~/.profile
            ```

        append: 
            ```shell
            export PATH=$PATH:/usr/local/go/bin
            export PATH=$PATH:~/go/bin
            ```

        apply changes:  
            ```shell
            source ~/.profile
            ```

        verify:
            ```shell
            go version
            ```

### Install Kind
    1. Install
            ```shell
            go install sigs.k8s.io/kind@v0.17.0
            ```

    2. Verify
            ```shell
            kind version
            ```

### Install Kubectl
    1. Install

            ```shell
            curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

            sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
            ```

    2. Verify

            ```shell
            kubectl version
            ```

### Install Helm
    1. Install

            ```shell
            curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
            chmod 700 get_helm.sh
            ./get_helm.sh
            ```

    2. Verify

            ```shell
            helm version
            ```

### Install Helm Diff Plugin
    1. Install
            ```shell
            helm plugin install https://github.com/databus23/helm-diff
            ```

    2. Verify
            ```shell
            diff --version
            ```

### Install Helmfile Plugin
    1. Install

            ```shell
            curl -OL https://github.com/helmfile/helmfile/releases/download/v0.150.0/helmfile_0.150.0_linux_amd64.tar.gz

            tar -zxvf helmfile_0.150.0_linux_amd64.tar.gz

            sudo mv helmfile /usr/local/bin
            ```

    2. Verify

            ```shell
            helmfile version
            ```

### Install JDK 8

    1. Install SDKMAN

            ```shell
            sudo apt install zip unzip
            curl -s "https://get.sdkman.io" | bash
            ```

    2. Install Java 8

            ```shell
            sdk install java 8.0.362-zulu
            ```

    3. Verify

            ```shell
            java -version
            ```

### Platform Setup
    Follow https://carbynestack.io/getting-started/platform-setup

### Stack Deployment
    Follow https://carbynestack.io/getting-started/deployment/