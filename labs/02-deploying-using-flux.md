# Deploying using Flux

Flux is a tool that automatically ensures that the state of a cluster matches the config in git. It uses an operator in the cluster to trigger deployments inside Kubernetes, which means you don't need a separate CD tool. It monitors all relevant image repositories, detects new images, triggers deployments and updates the desired running configuration based on that (and a configurable policy).

Flux is our tool for implementing GitOps.

In this lab you will learn how to:

* Deploy an artifact using Flux
* Trigger Flux to force an update to an artifact
* Configure your deployment so that Flux deploys your artifacts conditionally

