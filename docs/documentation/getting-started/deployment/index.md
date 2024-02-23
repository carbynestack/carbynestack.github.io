# Deployment

This guide describes how to deploy a two-party Carbyne Stack Virtual Cloud on
your local machine using [kind](https://kind.sigs.k8s.io/) clusters.

We support two distinct deployment methods:

1. [Manual deployment](manual) using mainly `kubectl` and the shell is presently
   the most instructive, thoroughly tested, and flexible method for deploying
   Carbyne Stack.
2. [Automated deployment](infrastructure-as-code) using Terraform CDKTF is a
   very convenient way to deploy Carbyne Stack, but the implementation
   is still early stage and should be considered experimental. Not all
   configuration options are supported as of today, e.g., using a secure offline
   phase implementation.
