# Solving the Millionaires Problem

In this guide, you will learn how to solve the millionaires problem using an
MPC function deployed on Carbyne Stack.

## Prerequisites

You need a deployed Carbyne Stack Virtual Cloud and the Carbyne Stack CLI.
Please see the [Platform Setup Guide](../platform-setup) and the
[Deployment Guide](../deployment) for instructions on how to get hold of
these.

In addition, this guide assumes that you have the following tools installed:

- Java 11 (newer versions will not work)

## The Billionaires Problem

We use a variation of Andrew Yao's [Millionaires' Problem](https://en.wikipedia.org/wiki/Yao%27s_Millionaires%27_problem)
that is the secure multi-party computation problem of deciding which of two
millionaires, Alice and Bob, is richer without revealing their actual wealth.
Since the conception of the problem, the world has moved on and we better speak
today not of two millionaires but of two (completely fictional) billionaires
called Jeff and Elon.

In the following, we describe how to solve this problem using Carbyne Stack. To
see how things work, let's put ourselves in Elon's shoes.

### Providing the Inputs

First, we upload the inputs into the Carbyne Stack
[Amphora Secret Store](https://github.com/carbynestack/amphora). The inputs are
the billionaires' net worth in billions. Note that this obviously has to be done
in a private way by Jeff and Elon in a real-world setting, simplified here by
logging in as individual users.

The first secret will be uploaded with the identity of Jeff. To do so perform
the following commands and login as Jeff using the following credentials
<table>
  <tr>
    <td>E-Mail:</td>
    <td>jeff@carbynestack.io</td>
  </tr><tr>
    <td>Password:</td>
    <td>86KIo6<]!/V=</td>
  </tr>
</table>

!!! info
    If you have recently authenticated yourself to the VCPs, your previous
    session might still be cached using a browser cookie.

    If you are not prompted for your credentials and not logged in as the 
    desired user, please make sure to clear recent browser cache or cookies,
    or re-open the tabs in private mode.

```shell
java -jar cs.jar login
# Create a secret representing Jeff's net worth (note that we work with 
# billion USD here)
export JEFFS_NET_WORTH_ID=$(java -jar cs.jar amphora create-secret 177 -t billionaire=Jeff -t accessPolicy=carbynestack.def -t authorizedPrograms=ephemeral-generic)
```

Next log in as Elon to perform the remaining steps of the tutorial. The
credentials for the development user Elon are as follows:
<table>
  <tr>
    <td>E-Mail:</td>
    <td>elon@carbynestack.io</td>
  </tr><tr>
    <td>Password:</td>
    <td>2#Tv91*d-Z,M</td>
  </tr>
</table>

Please read the info box above if you are having troubles logging in as a different user.

```shell
java -jar cs.jar login
# And a secret for Elon
export ELONS_NET_WORTH_ID=$(java -jar cs.jar amphora create-secret 151 -t billionaire=Elon -t accessPolicy=carbynestack.def -t authorizedPrograms=ephemeral-generic)
```

We can check the secrets have been created using:

```shell
java -jar cs.jar amphora get-secrets
```

The output should resemble the following:

!!! note
    The output you see will differ wrt. identifiers and the `creation-date` tag.
    <br>
    Nevertheless, it will output both secrets, uploaded by Elon and Jeff even
    though we are authenticated as Elon. However, accessing secrets to which a 
    user is not authorized - as defined by the policy attached to the secret -
    will result in an `Unauthorized` error.

```shell
09261bd8-b668-483a-8e4b-6095829f0ffa
  creation-date -> 1734525549919
  accessPolicy -> carbynestack.def
  billionaire -> Jeff
  owner -> 366e17fc-8aa9-47de-9a3c-80b3793dc27e
  authorizedPrograms -> ephemeral-generic
23602d07-7381-49a0-9528-e116ff290efc
  owner -> ca15ffbb-a140-49e9-88f1-f442397bad8f
  billionaire -> Elon
  creation-date -> 1734526180174
  authorizedPrograms -> ephemeral-generic
  accessPolicy -> carbynestack.def
```

### Invoke the Billionaires Function

Elon is eager to see whether he ranks first in the list of the richest people in
the world. To check that using the Carbyne Stack platform, a well-known global
media company which regularly publishes a list of the richest people in the
world came up with the following [MP-SPDZ](https://github.com/data61/MP-SPDZ)
program that does the job:

```shell
cat << 'EOF' > billionaires.mpc
# Prologue to read in the inputs
port=regint(10000)
listen(port)
socket_id = regint()
acceptclientconnection(socket_id, port)
v = sint.read_from_socket(socket_id, 2)

# The logic
first_billionaires_net_worth = v[0]
second_billionaires_net_worth= v[1]
result = first_billionaires_net_worth < second_billionaires_net_worth

# Epilogue to return the outputs 
resp = Array(1, sint)
resp[0] = result
sint.write_to_socket(socket_id, resp)
EOF
```

The program expects two inputs and does a simple comparison between them. The
result written as a secret to Amphora at the end of the program is either `1` in
case the first input is less than the second or `0` otherwise.

Elon invokes the program using the
[Ephemeral Serverless Compute Service](https://github.com/carbynestack/ephemeral)
as follows:

```shell
export RESULT_ID=$(cat billionaires.mpc | java -jar cs.jar ephemeral execute \
  -i $JEFFS_NET_WORTH_ID \
  -i $ELONS_NET_WORTH_ID \
  ephemeral-generic.default \
  | tail -n +2 \
  | sed 's/[][]//g')
```

The CLI spits out a list of identifiers of Amphora Secrets generated by the
program. The snippet above extracts the single identifier emitted in this
example and stores it in the `RESULT_ID` shell variable.

Using this identifier Elon can inspect the result of the execution using:

```shell
java -jar cs.jar amphora get-secret $RESULT_ID
```

The output being `0` tells Elon that unfortunately Jeff is still the alpha:

```shell
[0]
  gameID -> 56effe1e-d85c-4af0-8894-846f3b62e4db
  creation-date -> 1734526737566
  derived-from -> 56580701-5334-4bf4-a1c0-354d51a6e6d8, 64050fa9-f48d-4ec1-838a-6fbcf354827d
  owner -> 64050fa9-f48d-4ec1-838a-6fbcf354827d
```

After buying a bunch of the fabulous new (and completely fictional) _Carbyne
Stack Altcoins_ and posting a tweet that one of his companies will accept that
coin for payments in the future, Elon wants to see whether he is finally in the
pole position. He deletes his old and submits his new net worth and triggers the
evaluation of the Billionaires Problem logic again using:

```shell
java -jar cs.jar amphora delete-secrets $ELONS_NET_WORTH_ID
export ELONS_NET_WORTH_ID=$(java -jar cs.jar amphora create-secret 179 -t billionaire=Elon -t accessPolicy=carbynestack.def -t authorizedPrograms=ephemeral-generic)
export RESULT_ID=$(cat billionaires.mpc | java -jar cs.jar ephemeral execute \
  -i $JEFFS_NET_WORTH_ID \
  -i $ELONS_NET_WORTH_ID \
  ephemeral-generic.default \
  | tail -n +2 \
  | sed 's/[][]//g')
java -jar cs.jar amphora get-secret $RESULT_ID
```

This time the execution results in a '1', which creates a pleasantly warm
feeling in Elon's chest.

## Next Steps

This simple tutorial showcased only a small subset of the Carbyne Stack's
capabilities. Feel free to explore the possibilities yourself by playing around
with the CLI.
