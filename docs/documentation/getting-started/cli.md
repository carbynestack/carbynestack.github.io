# Configuring the CLI

In this guide, you will learn how to download and configure the Carbyne Stack
CLI that can be used to interact with a virtual cloud from the command line.

!!! info
    This guide expects Carbyne Stack to be deployed in a _local_ two player
    setting using kind clusters as described in the
    [manual deploymend guide](../deployment/manual). Cluster names as used for
    connecting to the clusters may be different for individual deployments.

1. Install the CLI using:

    ```shell
    export CLI_VERSION=0.6.0
    curl -o cs.jar -L https://github.com/carbynestack/cli/releases/download/cli-v$CLI_VERSION/cli-$CLI_VERSION.jar
    ```

1. Export the IP addresses of the Istio _Ingress Gateways_ from the
   [deployment tutorial](../deployment):

    ```shell
    export APOLLO_FQDN="172.18.1.128.sslip.io"
    export STARBUCK_FQDN="172.18.2.128.sslip.io"
    export PROTOCOL="https" # set to http if TLS is disabled
    ```

1. Export the Thymus OAuth2 client IDs for both VCPs.

    !!! info
        Thymus automatically registers an OAuth2 client for authentication with
        the Carbyne Stack VCP and stores its ID as a k8s secret called
        `thymus-client-secret`. For more information about OAuth2 clients see the
        [Ory Hydry documentation](https://www.ory.sh/docs/hydra/guides/oauth2-clients).

    ```shell
    kubectl config use-context kind-apollo
    export APOLLO_OAUTH2_CLIENT_ID=$(kubectl get secret thymus-client-secret --template {{.data.CLIENT_ID}} | base64 -d)
    kubectl config use-context kind-starbuck
    export STARBUCK_OAUTH2_CLIENT_ID=$(kubectl get secret thymus-client-secret --template {{.data.CLIENT_ID}} | base64 -d)
    ```

1. Next, configure the CLI to talk to the virtual cloud you just deployed by
   creating a matching CLI configuration file in `~/.cs` using:

    ```shell
    mkdir -p ~/.cs
    cat <<EOF | envsubst > ~/.cs/config
    {
      "prime" : 198766463529478683931867765928436695041,
      "r" : 141515903391459779531506841503331516415,
      "noSslValidation" : true,
      "trustedCertificates" : [ ],
      "providers" : [ {
        "amphoraServiceUrl" : "$PROTOCOL://$APOLLO_FQDN/amphora",
        "castorServiceUrl" : "$PROTOCOL://$APOLLO_FQDN/castor",
        "ephemeralServiceUrl" : "$PROTOCOL://$APOLLO_FQDN/",
        "thymusServiceUrl" : "$PROTOCOL://$APOLLO_FQDN/iam/policies",
        "oauth2ClientId": "$APOLLO_OAUTH2_CLIENT_ID",
        "oauth2AuthEndpointUri": "$PROTOCOL://$APOLLO_FQDN/iam/oauth/oauth2/auth",
        "oauth2TokenEndpointUri": "$PROTOCOL://$APOLLO_FQDN/iam/oauth/oauth2/token",
        "oauth2CallbackUrl": "http://127.0.0.1:32768/callback",
        "id" : 1,
        "baseUrl" : "$PROTOCOL://$APOLLO_FQDN/"
      }, {
        "amphoraServiceUrl" : "$PROTOCOL://$STARBUCK_FQDN/amphora",
        "castorServiceUrl" : "$PROTOCOL://$STARBUCK_FQDN/castor",
        "ephemeralServiceUrl" : "$PROTOCOL://$STARBUCK_FQDN/",
        "thymusServiceUrl" : "$PROTOCOL://$STARBUCK_FQDN/iam/policies",
        "oauth2ClientId": "$STARBUCK_OAUTH2_CLIENT_ID",
        "oauth2AuthEndpointUri": "$PROTOCOL://$STARBUCK_FQDN/iam/oauth/oauth2/auth",
        "oauth2TokenEndpointUri": "$PROTOCOL://$STARBUCK_FQDN/iam/oauth/oauth2/token",
        "oauth2CallbackUrl": "http://127.0.0.1:32768/callback",
        "id" : 2,
        "baseUrl" : "$PROTOCOL://$STARBUCK_FQDN/"
      } ],
      "rinv" : 133854242216446749056083838363708373830
    }
    EOF
    ```

    Alternatively, you can use the CLI tool itself to do the configuration by
    providing the respective values (as seen above in the HEREDOC) when prompted
    using:

    ```shell
    java -jar cs.jar configure
    ```

1. Log in to the VCPs

    With the user-facing endpoints being secured using _OAuth2.0_ and _OpenID
    Connect_, it is required to authenticate to the VCPs. This can be done
    using:

    ```shell
    java -jar cs.jar login
    ```

    !!! info
        The command above will open a browser window for each VCPs and prompt for
        authentication.

        The development setup as described in the 
        [deployment tutorial](../deployment) will automatically register two 
        demo users as follows:
        
        | E-Mail | Password |
        | ------ | -------- |
        | elon@carbynestack.io | 2#Tv91*d-Z,M |
        | jeff@carbynestack.io | 86KIo6<]!/V= |

    !!! warning
        If you register individual users, you must ensure that the users are
        registered in all VCPs with the same e-mail address. Passwords can be
        set individually.

1. [_Optional_] Verify the configuration

    You can verify that the configuration works by fetching telemetry data from
    castor using:

    !!! attention
        Replace `<#>` with either `1` for the `apollo` cluster or `2` for the
        `starbuck` cluster.

    ```shell
    java -jar cs.jar castor get-telemetry <#>
    ```

    !!! warning
        Before actually using the Carbyne Stack Virtual Cloud please make sure
        that enough correlated randomness has been generated by Klyshko in the
        background. You can verify that this is the case by fetching telemetry
        data via

        ```shell
        java -jar cs.jar castor get-telemetry 1
        ```

    For the next part of the tutorial, it is recommended to wait until
    Klyshko has generated at least one batch of tuples for each tuple type.
