# Platform Setup Guide

This guide describes how to prepare K8s clusters using `kind` that are suitable
for deploying a two-party Carbyne Stack Virtual Cloud. After completing the
steps described below, you should have two kind K8s clusters called `apollo` and
`starbuck` with the following pods deployed:

```shell
kubectl get pods -A
NAMESPACE            NAME                                             READY   STATUS    RESTARTS   AGE
default              istio-operator-7cf558f694-nmm9b                  1/1     Running   0          10m
default              knative-operator-f6c995cb7-bfpfq                 1/1     Running   0          7m42s
default              operator-webhook-54bfbccdf9-7fc5w                1/1     Running   0          7m42s
default              postgres-operator-5b6b676464-m22w2               1/1     Running   0          7m17s
istio-system         istio-ingressgateway-5b57d7fd4f-qhrkp            1/1     Running   0          9m30s
istio-system         istiod-5f7d759f66-h29xw                          1/1     Running   0          9m42s
knative-serving      activator-6f7db96dc5-54lqv                       1/1     Running   0          7m13s
knative-serving      autoscaler-d9c7cc55c-qbc4n                       1/1     Running   0          7m12s
knative-serving      controller-6456dbd66f-nd6nl                      1/1     Running   0          7m12s
knative-serving      domain-mapping-769cfb7fb5-d2mwc                  1/1     Running   0          7m11s
knative-serving      domainmapping-webhook-54f4564c48-86z6d           1/1     Running   0          7m11s
knative-serving      net-istio-controller-d5977cfbb-m7ttl             1/1     Running   0          7m8s
knative-serving      net-istio-webhook-5dff88bdb8-ktbgq               1/1     Running   0          7m8s
knative-serving      webhook-6447645d8d-6k4xp                         1/1     Running   0          7m10s
kube-system          coredns-787d4945fb-c8dkg                         1/1     Running   0          10m
kube-system          coredns-787d4945fb-lgjjg                         1/1     Running   0          10m
kube-system          etcd-starbuck-control-plane                      1/1     Running   0          10m
kube-system          kindnet-z7vwz                                    1/1     Running   0          10m
kube-system          kube-apiserver-starbuck-control-plane            1/1     Running   0          10m
kube-system          kube-controller-manager-starbuck-control-plane   1/1     Running   0          10m
kube-system          kube-proxy-t5259                                 1/1     Running   0          10m
kube-system          kube-scheduler-starbuck-control-plane            1/1     Running   0          10m
local-path-storage   local-path-provisioner-c8855d4bb-zhl9t           1/1     Running   0          10m
metallb-system       metallb-controller-777d84cdd5-2jjkd              1/1     Running   0          8m44s
metallb-system       metallb-speaker-vmqj4                            1/1     Running   0          8m44s
```

## Prerequisites

!!! warning
    Carbyne Stack has been tested using the **exact versions** of the tools
    specified below. Deviating from this _battle tested_ configuration may
    create all kinds of issues.

    Do not forget to perform the [post installation steps](https://docs.docker.com/engine/install/linux-postinstall/)
    for Docker.

!!! info
    You'll need at least 3 GB of memory and 1 CPU core per kind cluster to
    deploy Carbyne Stack. Depending on the actual workloads you are going to
    deploy, these numbers can be considerably higher.

- [Docker Engine](https://docs.docker.com/engine/install/ubuntu/) v23.0.1
- [Kind](https://kind.sigs.k8s.io/) v0.17.0
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/) v1.26.1
- [Helm](https://helm.sh/docs/intro/install/) v3.11.1
- [Helmfile](https://github.com/roboll/helmfile) v0.142.0
- [Helm Diff Plugin](https://github.com/databus23/helm-diff) v3.1.3
- [OpenJDK](https://openjdk.java.net/install/) 11

For installation instructions for these tools see the
[Prerequisites](../prerequisites) section.

## Setting up the Clusters

### Kind Clusters

You will need two Carbyne Stack Virtual Cloud Providers deployed to separate K8s
clusters to complete this getting started guide. The clusters are called
`apollo` and `starbuck`. You can use the `--name <name>` option to launch
a kind cluster with K8s context name `kind-<name>`, as follows:

=== "Apollo"

    ```shell
    kind create cluster --name apollo --image kindest/node:v1.26.6
    ```

=== "Starbuck"

    ```shell
    kind create cluster --name starbuck --image kindest/node:v1.26.6
    ```

You can switch between the clusters easily using:

=== "Apollo"

    ```shell
    kubectl config use-context kind-apollo
    ```

=== "Starbuck"

    ```shell
    kubectl config use-context kind-starbuck
    ```

!!! important
    Complete the remaining steps of this guide for the `apollo` cluster and then
    repeat for `starbuck`.

### Istio

1. Install
   the [Istio Operator](https://istio.io/latest/docs/setup/install/operator/)
   v1.17.0 using:

    ```shell
    curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.17.0 TARGET_ARCH=x86_64 sh -
    helm install istio-operator istio-1.17.0/manifests/charts/istio-operator \
      --set operatorNamespace=istio-operator \
      --set watchedNamespaces="istio-system" \
      --set hub="docker.io/istio" \
      --set tag="1.17.0"
    ```

1. Create an Istio Control Plane in a dedicated namespace using:

    ```shell
    cat <<EOF > istio-control-plane.yaml
    apiVersion: v1
    kind: Namespace
    metadata:
      name: istio-system
    ---
    apiVersion: install.istio.io/v1alpha1
    kind: IstioOperator
    metadata:
      namespace: istio-system
      name: cs-istiocontrolplane
    spec:
      meshConfig:
        accessLogFile: /dev/stdout
      components:
        ingressGateways:
          - name: istio-ingressgateway
            enabled: true
            k8s:
              resources:
                requests:
                  cpu: 10m
                  memory: 40Mi
              service:
                ports:
                  ## You can add custom gateway ports in user values overrides,
                  # but it must include those ports since helm replaces.
                  # Note that AWS ELB will by default perform health checks on
                  # the first port on this list. Setting this to the health
                  # check port will ensure that health checks always work.
                  # https://github.com/istio/istio/issues/12503
                  - port: 15021
                    targetPort: 15021
                    name: status-port
                  - port: 80
                    targetPort: 8080
                    name: http2
                  - port: 443
                    targetPort: 8443
                    name: https
                  - port: 31400
                    targetPort: 31400
                    name: tcp
                    # This is the port where sni routing happens
                  - port: 15443
                    targetPort: 15443
                    name: tls
                  - port: 30000
                    name: ephemeral-mpc-engine-port-0
                  - port: 30001
                    name: ephemeral-mpc-engine-port-1
                  - port: 30002
                    name: ephemeral-mpc-engine-port-2
                  - port: 30003
                    name: ephemeral-mpc-engine-port-3
                  - port: 30004
                    name: ephemeral-mpc-engine-port-4
        pilot:
          k8s:
            env:
              - name: PILOT_TRACE_SAMPLING
                value: "100"
            resources:
              requests:
                cpu: 10m
                memory: 100Mi
      values:
        global:
          proxy:
            resources:
              requests:
                cpu: 10m
                memory: 40Mi
        pilot:
          autoscaleEnabled: false
        gateways:
          istio-egressgateway:
            autoscaleEnabled: false
          istio-ingressgateway:
            autoscaleEnabled: false
    EOF
    kubectl apply -f istio-control-plane.yaml
    ```

### MetalLB

1. Install [MetalLB](https://metallb.universe.tf/) v0.13.9 using:

    ```shell
    helm repo add metallb https://metallb.github.io/metallb
    kubectl create namespace metallb-system
    cat <<EOF | envsubst > metallb-values.yaml
    apiVersion: v1
    kind: Namespace
    metadata:
      labels:
        pod-security.kubernetes.io/audit: privileged
        pod-security.kubernetes.io/enforce: privileged
        pod-security.kubernetes.io/warn: privileged
      name: metallb-system
    EOF
    helm install metallb metallb/metallb --version 0.13.9 -n metallb-system -f metallb-values.yaml
    ```

1. Configure MetalLB using:

    === "Apollo"

        ```shell
        export SUBNET=172.18.1.255/25
        cat <<EOF | envsubst > metallb.yaml
        apiVersion: metallb.io/v1beta1
        kind: IPAddressPool
        metadata:
          name: default
          namespace: metallb-system
        spec:
          addresses:
          - ${SUBNET}
        ---
        apiVersion: metallb.io/v1beta1
        kind: L2Advertisement
        metadata:
          name: empty
          namespace: metallb-system
        EOF
        kubectl apply -f metallb.yaml
        ```

    === "Starbuck"

        ```shell
        export SUBNET=172.18.2.255/25
        cat <<EOF | envsubst > metallb.yaml
        apiVersion: metallb.io/v1beta1
        kind: IPAddressPool
        metadata:
          name: default
          namespace: metallb-system
        spec:
          addresses:
          - ${SUBNET}
        ---
        apiVersion: metallb.io/v1beta1
        kind: L2Advertisement
        metadata:
          name: empty
          namespace: metallb-system
        EOF
        kubectl apply -f metallb.yaml
        ```

1. Wait until an external IP has been assigned to the Istio Ingress Gateway by
   MetalLB:

    ```shell
    kubectl get services --namespace istio-system istio-ingressgateway -w
    ```

The public IP eventually appears in column `EXTERNAL-IP`.

1. Export the external IP for later use:

    ```shell
    export EXTERNAL_IP=$(kubectl get services --namespace istio-system istio-ingressgateway --output jsonpath='{.status.loadBalancer.ingress[0].ip}')
    ```

### Knative

1. Install the
   [Knative Operator](https://knative.dev/docs/install/operator/knative-with-operators/)
   v1.8.2 using:

    ```shell
    kubectl apply -f https://github.com/knative/operator/releases/download/knative-v1.8.2/operator.yaml
    ```

1. Create a namespace for Knative Serving using:

    ```shell
    kubectl create namespace knative-serving
    ```

1. Install the patched Knative Serving component with a
   [sslip.io](https://sslip.io/) custom domain using:

    ```shell
    cat <<EOF | envsubst > knative-serving.yaml
    apiVersion: operator.knative.dev/v1beta1
    kind: KnativeServing
    metadata:
      name: knative-serving
      namespace: knative-serving
    spec:
      version: 1.8.2
      manifests:
        - URL: https://github.com/carbynestack/serving/releases/download/v1.8.2-multiport-patch/serving-crds.yaml
        - URL: https://github.com/carbynestack/serving/releases/download/v1.8.2-multiport-patch/serving-core.yaml
        - URL: https://github.com/knative/net-istio/releases/download/v1.8.2/release.yaml
        - URL: https://github.com/knative/net-certmanager/releases/download/v1.8.2/release.yaml
      config:
         domain:
            ${EXTERNAL_IP}.sslip.io: ""
         defaults:
            max-revision-timeout-seconds: "36000"
    EOF
    kubectl apply -f knative-serving.yaml
    ```

    The configuration above will also increase Knative's default
    [max-revision-timeout-seconds](https://knative.dev/v1.9-docs/serving/configuration/config-defaults/#revision-timeout-seconds)
    from `600` to `36000` seconds (10h). This is required as ephemeral
    computations are executed as so-called Knative activations and are therefore
    subject to its configuration.

### Postgres Operator

Deploy the
[Zalando Postgres operator](https://github.com/zalando/postgres-operator) v1.9.0
using:

```shell
curl -sL https://github.com/zalando/postgres-operator/archive/refs/tags/v1.9.0.tar.gz | tar -xz
helm install postgres-operator postgres-operator-1.9.0/charts/postgres-operator
```

## Clean Up

If you no longer need the cluster you can tear it down using:

=== "Apollo"

    ```shell
    kind delete cluster --name apollo
    ```

=== "Starbuck"

    ```shell
    kind delete cluster --name starbuck
    ```

## Troubleshooting

### OpenVPN

In case you use OpenVPN and encounter an error message when launching a kind
cluster like

```shell
ERROR: failed to create cluster: failed to ensure docker network: command "docker network create -d=bridge -o com.docker.network.bridge.enable_ip_masquerade=true -o com.docker.network.driver.mtu=1500 --ipv6 --subnet fc00:f853:ccd:e793::/64 kind" failed with error: exit status 1
Command Output: Error response from daemon: could not find an available, non-overlapping IPv4 address pool among the default
```

follow the advice given [here](https://stackoverflow.com/a/45694531).

### kind

In case one of your kind clusters doesn't start all pods, there might an issue
with limited inotify resources on Ubuntu. You can check this by printing the
logs of the faulty pods by, e.g.:

```shell
kubectl logs -n kube-system <pod-name>
```

If there is an output similar to this

```shell
 "command failed" err="failed complete: too many open files"
```

follow the advice given [here](https://kind.sigs.k8s.io/docs/user/known-issues/#pod-errors-due-to-too-many-open-files).
