# Stack Deployment Guide

This guide describes how to set up a Carbyne Stack Virtual Cloud (VC) consisting
of two Virtual Cloud Providers (VCP).

## Prerequisites

!!! warning
    Carbyne Stack has been tested using the **exact versions** of the tools
    specified below. Deviating from this _battle tested_ configuration may
    create all kinds of issues.

- [Helmfile](https://github.com/roboll/helmfile) v0.142.0
- [Helm](https://helm.sh/) v3.11.1
- [Helm Diff Plugin](https://github.com/databus23/helm-diff) v3.1.3

In addition, this guide assumes you have access to two properly configured K8s
clusters (herein referred to as `apollo` and `starbuck`) with the following
components:

- Kubernetes v1.26.0
- Istio v1.17.0
- MetalLB v0.13.9
- Knative v1.8.2
- Zalando Postgres Operator v1.9.0

Throughout the remainder of this guide, we assume that you have set up local
clusters using the kind tool as described in the
[Platform Setup](../platform-setup) guide.

## Virtual Cloud Deployment

!!! tip
    In case you are on a slow internet connection, you can use

    ```shell
    kind load docker-image <image> --name <cluster-name>
    ```

    to load images from your local docker registry into the kind clusters. This
    way you have to download the images only once and then reuse them across
    VCP deployments.

1. Clone the [carbynestack repository](https://github.com/carbynestack/carbynestack)
   and descend into the repository root directory using:

    === "HTTP"

        ```shell
        git clone https://github.com/carbynestack/carbynestack.git
        cd carbynestack/deployments
        ```

    === "SSH"

        ```shell
        git clone git@github.com:carbynestack/carbynestack.git
        cd carbynestack/deployments
        ```

1. Checkout Carbyne Stack SDK version 0.8.1 using:

    ```shell
    git checkout sdk-v0.8.1
    ```

1. Before deploying the virtual cloud providers make some common configuration
   available using:

    !!! attention
        Replace `172.18.1.128` and `172.18.2.128` with the load balancer IPs
        assigned to the Istio Ingress Gateway by MetalLB (see the
        [Platform Setup](../platform-setup) guide).

    ```shell
    export APOLLO_IP="172.18.1.128"
    export APOLLO_FQDN="${APOLLO_IP}.sslip.io"
    export STARBUCK_IP="172.18.2.128"
    export STARBUCK_FQDN="${STARBUCK_IP}.sslip.io"
    export PARTNER_COUNT=2
    export PARTNER_FQDN_0=$APOLLO_FQDN
    export PARTNER_FQDN_1=$STARBUCK_FQDN
    export ETCD_MASTER_URL=$APOLLO_FQDN
    export ETCD_MASTER_IP=$APOLLO_IP
    export RELEASE_NAME=cs
    export NO_SSL_VALIDATION=true
    export TLS_ENABLED=true # Enabled by default, set to false to disable
    export PROTOCOL=https
    ```

1. Configure the _Correlated Randomness Generator_ (CRG) used by Klyshko

    === "Insecure"

        !!! danger
            **_DO NOT USE THIS IN A PRODUCTION SETTING_**.
    
        By default, correlated randomness is generated using a cheap but
        *insecure* fake offline phase implementation. Using this CRG is
        recommended for development and demo purposes only.

    === "Secure"

        !!! warning
            In this configuration, CR generation will consume a substantial
            amount of resources (CPU and bandwidth). In addition, the offline
            phase docker container used is platform-dependent. This means
            execution may fail on your platform (see 
            [here](https://github.com/carbynestack/klyshko/issues/78) for more 
            information).
    
        Carbyne Stack comes with an *experimental* correlated randomness
        generator based on the MP-SPDZ CowGear offline phase implementation. To
        enable this CRG, invoke
    
        ```shell
        export KLYSHKO_GENERATOR_IMAGE_REPOSITORY=carbynestack/klyshko-mp-spdz-cowgear
        export KLYSHKO_GENERATOR_IMAGE_TAG=0.2.0
        ```

        before you proceed.

1. Configure TLS for secure communication to, and between the VCPs:

    As secrets must exist in the namespace of the proxy and components using
    the certificates, they are created in the `istio-system` namespace and then
    copied to the `knative-serving` and `default` namespace.

    <!-- markdownlint-disable MD013 -->
    ```shell
     # Create X.509 certificates
     mkdir -p certs
     openssl req -x509 -newkey rsa:4096 -keyout certs/apollo_key.pem -out certs/apollo_cert.pem -days 365 -nodes -subj "/CN=${APOLLO_FQDN}" -addext "subjectAltName=DNS:${APOLLO_FQDN},IP:${APOLLO_IP}"
     openssl req -x509 -newkey rsa:4096 -keyout certs/starbuck_key.pem -out certs/starbuck_cert.pem -days 365 -nodes -subj "/CN=${STARBUCK_FQDN}" -addext "subjectAltName=DNS:${STARBUCK_FQDN},IP:${STARBUCK_IP}"

     # Create kubernetes secrets using the generated keys and certificates
     kubectl config use-context kind-apollo
     kubectl create secret generic apollo-tls-secret-generic -n istio-system --from-file=tls.key=certs/apollo_key.pem --from-file=tls.crt=certs/apollo_cert.pem --from-file=cacert=certs/starbuck_cert.pem
     kubectl get secret apollo-tls-secret-generic -n istio-system -o yaml | sed 's/namespace: istio-system/namespace: knative-serving/' | kubectl apply -n knative-serving -f -
     kubectl get secret apollo-tls-secret-generic -n istio-system -o yaml | sed 's/namespace: istio-system/namespace: default/' | kubectl apply -n default -f -
     kubectl config use-context kind-starbuck
     kubectl create secret generic starbuck-tls-secret-generic -n istio-system --from-file=tls.key=certs/starbuck_key.pem --from-file=tls.crt=certs/starbuck_cert.pem --from-file=cacert=certs/apollo_cert.pem
     kubectl get secret starbuck-tls-secret-generic -n istio-system -o yaml | sed 's/namespace: istio-system/namespace: knative-serving/' | kubectl apply -n knative-serving -f -
     kubectl get secret starbuck-tls-secret-generic -n istio-system -o yaml | sed 's/namespace: istio-system/namespace: default/' | kubectl apply -n default -f -
    ```
    <!-- markdownlint-enable MD013 -->

1. Patch Knative to secure its gateway using TLS with the generated certificates:

    <!-- markdownlint-disable MD013 -->
    ```shell
    kubectl config use-context kind-apollo
    kubectl patch gateway knative-ingress-gateway --namespace knative-serving --type=json --patch="[{\"op\": \"add\", \"path\": \"/spec/servers/0/tls\", \"value\": {\"mode\": \"SIMPLE\", \"credentialName\": \"apollo-tls-secret-generic\"}}]"
    kubectl config use-context kind-starbuck
    kubectl patch gateway knative-ingress-gateway --namespace knative-serving --type=json --patch="[{\"op\": \"add\", \"path\": \"/spec/servers/0/tls\", \"value\": {\"mode\": \"SIMPLE\", \"credentialName\": \"starbuck-tls-secret-generic\"}}]"
    ```
    <!-- markdownlint-enable MD013 -->

1. Launch the `starbuck` VCP using:

    ```shell
    export PLAYER_ID=1
    export TLS_SECRET_NAME=starbuck-tls-secret-generic
    kubectl config use-context kind-starbuck
    helmfile sync --set thymus.users.enabled=true
    ```

1. Launch the `apollo` VCP using:

    ```shell
    export PLAYER_ID=0
    export TLS_SECRET_NAME=apollo-tls-secret-generic
    kubectl config use-context kind-apollo
    helmfile sync --set thymus.users.enabled=true
    ```

1. Wait until all pods in both clusters are in the `ready` state.

You now have a fully functional Carbyne Stack Virtual Cloud at your hands.

## Teardown the Virtual Cloud

You can tear down the Virtual Cloud by tearing down the Virtual Cloud Providers
using:

```shell
for var in apollo starbuck
do
  kubectl config use-context kind-$var
  helmfile destroy
done
```
