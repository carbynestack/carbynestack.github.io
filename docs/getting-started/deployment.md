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

1. Checkout out the [carbynestack repository](https://github.com/carbynestack/carbynestack)
   and descend into the repository root directory using:

    === "HTTP"

        ```shell
        git clone https://github.com/carbynestack/carbynestack.git
        cd carbynestack
        ```

    === "SSH"

        ```shell
        git clone git@github.com:carbynestack/carbynestack.git
        cd carbynestack
        ```

1. Before deploying the virtual cloud providers make some common configuration
   available using:

    !!! attention
        Replace `172.18.1.128` and `172.18.2.128` with the load balancer IPs
        assigned to the Istio Ingress Gateway by MetalLB (see the
        [Platform Setup](../platform-setup) guide).

    ```shell
    export APOLLO_FQDN="172.18.1.128.sslip.io"
    export STARBUCK_FQDN="172.18.2.128.sslip.io"
    export RELEASE_NAME=cs
    export DISCOVERY_MASTER_HOST=$APOLLO_FQDN
    export NO_SSL_VALIDATION=true
    ```

1. Launch the `starbuck` VCP using:

    ```shell
    export FRONTEND_URL=$STARBUCK_FQDN
    export IS_MASTER=false
    export AMPHORA_VC_PARTNER_URI=http://$APOLLO_FQDN/amphora
    kubectl config use-context kind-starbuck
    helmfile apply
    ```

1. Launch the `apollo` VCP using:

    ```shell
    export FRONTEND_URL=$APOLLO_FQDN
    export IS_MASTER=true
    export AMPHORA_VC_PARTNER_URI=http://$STARBUCK_FQDN/amphora
    export CASTOR_SLAVE_URI=http://$STARBUCK_FQDN/castor
    kubectl config use-context kind-apollo
    helmfile apply
    ```

1. Wait until all pods in both clusters are in the `ready` state.

## Preparing the Virtual Cloud

1. Carbyne Stack comes with a CLI that can be used to interact with a virtual
   cloud from the command line. Install the CLI using:

    ```shell
    export CLI_VERSION=0.2-SNAPSHOT-2336890983-14-a4260ab
    curl -o cs.jar -L https://github.com/carbynestack/cli/releases/download/$CLI_VERSION/cli-$CLI_VERSION-jar-with-dependencies.jar
    ```

2. Next configure the CLI to talk to the just deployed virtual cloud by creating
   a matching CLI configuration file in `~/.cs` using:

    ```shell
    mkdir -p ~/.cs
    cat <<EOF | envsubst > ~/.cs/config
    {
      "prime" : 198766463529478683931867765928436695041,
      "r" : 141515903391459779531506841503331516415,
      "noSslValidation" : true,
      "trustedCertificates" : [ ],
      "providers" : [ {
        "amphoraServiceUrl" : "http://$APOLLO_FQDN/amphora",
        "castorServiceUrl" : "http://$APOLLO_FQDN/castor",
        "ephemeralServiceUrl" : "http://$APOLLO_FQDN/",
        "id" : 1,
        "baseUrl" : "http://$APOLLO_FQDN/"
      }, {
        "amphoraServiceUrl" : "http://$STARBUCK_FQDN/amphora",
        "castorServiceUrl" : "http://$STARBUCK_FQDN/castor",
        "ephemeralServiceUrl" : "http://$STARBUCK_FQDN/",
        "id" : 2,
        "baseUrl" : "http://$STARBUCK_FQDN/"
      } ],
      "rinv" : 133854242216446749056083838363708373830
    }
    EOF
    ```

    Alternatively, you can use the CLI tool itself to do the configuration by
    providing the respective values (as seen above in the HEREDOC) when asked
    using:

    ```shell
    java -jar cs.jar configure
    ```

    You can verify that the configuration works by fetching telemetry data from
    castor using:

    !!! attention
        Replace `<#>` with either `1` for the `apollo` cluster or `2` for the
        `starbuck` cluster.

    ```shell
    java -jar cs.jar castor get-telemetry <#>
    ```

### Upload Offline Material

Before you can actually use the services provided by the Virtual Cloud, you have
to upload cryptographic material. As generating offline material is a very
time-consuming process, we provide pre-generated material.

!!! danger
    Using pre-generated offline material is not secure at all. **_DO NOT DO THIS
    IN A PRODUCTION SETTING_**.

1. Download and decompress the archive containing the material using:

    ```shell
    curl -O -L https://github.com/carbynestack/carbynestack/raw/9c0c17599ae08253398a000f2a23b3ded8611499/tuples/fake-crypto-material-0.2.zip
    unzip -d crypto-material fake-crypto-material-0.2.zip
    rm fake-crypto-material-0.2.zip
    ```

2. Upload and activate tuples using:

    !!! tip
        Adapt the `NUMBER_OF_CHUNKS` variable in the following snippet to tune
        the number of uploaded tuples. In case`NUMBER_OF_CHUNKS > 1` the **same**
        tuples are uploaded repeatedly.

    ```shell
    cat << 'EOF' > upload-tuples.sh
    #!/bin/bash
    SCRIPT_PATH="$( cd "$(dirname "$0")" ; pwd -P )"
    TUPLE_FOLDER=${SCRIPT_PATH}/crypto-material/
    cs="java -jar ${SCRIPT_PATH}/cs.jar"
    NUMBER_OF_CHUNKS=1

    tuples=(
      "BIT_GFP,2-p-128/Bits-p"
      "BIT_GF2N,2-2-40/Bits-2"
      "INPUT_MASK_GFP,2-p-128/Triples-p"
      "INPUT_MASK_GF2N,2-2-40/Triples-2"
      "INVERSE_TUPLE_GFP,2-p-128/Inverses-p"
      "INVERSE_TUPLE_GF2N,2-2-40/Inverses-2"
      "SQUARE_TUPLE_GFP,2-p-128/Squares-p"
      "SQUARE_TUPLE_GF2N,2-2-40/Squares-2"
      "MULTIPLICATION_TRIPLE_GFP,2-p-128/Triples-p"
      "MULTIPLICATION_TRIPLE_GF2N,2-2-40/Triples-2"
    )

    function uploadTuples {
       echo ${NUMBER_OF_CHUNKS}
       for t in ${tuples[@]}; do
          OLDIFS=$IFS
          IFS=','
          set -- $t
          type=$1
          tuple_file=$2
          IFS=$OLDIFS
          for (( i=0; i<${NUMBER_OF_CHUNKS}; i++ )); do
             local chunkId=$(uuidgen)
             echo "Uploading ${type} to http://${APOLLO_FQDN}/castor (Apollo)"
             $cs castor upload-tuple -f ${TUPLE_FOLDER}/${tuple_file}-P0 -t ${type} -i ${chunkId} 1
             local statusMaster=$?
             echo "Uploading ${type} to http://${STARBUCK_FQDN}/castor (Starbuck)"
             $cs castor upload-tuple -f ${TUPLE_FOLDER}/${tuple_file}-P1 -t ${type} -i ${chunkId} 2
             local statusSlave=$?
             if [[ "${statusMaster}" -eq 0 && "${statusSlave}" -eq 0 ]]; then
                $cs castor activate-chunk -i ${chunkId} 1
                $cs castor activate-chunk -i ${chunkId} 2
             else
                echo "ERROR: Failed to upload one tuple chunk - not activated"
             fi
          done
       done
    }

    uploadTuples
    EOF
    chmod 755 upload-tuples.sh
    ./upload-tuples.sh
    ```

3. You can verify that the uploaded tuples are now available for use by the
   Carbyne Stack services using:

    !!! attention
        Replace `<#>` with either `1` for the `apollo` cluster or `2` for the
        `starbuck` cluster.

    ```shell
    java -jar cs.jar castor get-telemetry <#>
    ```

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
