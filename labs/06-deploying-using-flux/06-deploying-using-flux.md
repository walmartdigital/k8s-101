# Deploying using Flux and exposing your artifacts to the Internet

In this lab, we will deploy our microservices in our Cloud.

Flux is a tool that automatically ensures that the state of a cluster matches the config in git. It uses an operator in the cluster to trigger deployments inside Kubernetes, which means you don't need a separate CD tool. It monitors all relevant image repositories, detects new images, triggers deployments and updates the desired running configuration based on that (and a configurable policy).

Flux is our tool for implementing GitOps in the Cloud.

In this lab you will learn how to:

* Use namespaces to isolate your artifacts from other users of the cluster
* Deploy an artifact the GitOps way using Flux

# Using namespaces 

Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces. Namespaces are a way to divide cluster resources between multiple users.

## Exercise: Create your own namespace

In the `hello-app.yaml` file, replace all occurences of `group-name` by a value that uniquely identifies your lab group in order to namespace your resources. Note that you will have to use the same namespace for all resources you create throughout this workshop.

```
vim hello-app.yaml
```

## Exercise: Deploy using Flux

On your local machine, clone the deployment repository for the frontend labs cluster (ask your instructor for the URL).

```
git clone https://<git-remote>/walmartdigital/frontend-k8s
```

Copy the `hello-app.yaml` file to the "deployments" folder, making sure to rename it so that it has a unique name which easily identifies your team, e.g., `bob-and-alice.yaml`.

```
cp <path-to-k8s-101-repo>/06-deploying-using-flux/hello-app.yaml <path-to-deployment-repo>/frontend-k8s/deployments/labs/eastus2/alice-and-bob.yaml
```

Replace the namespace of all K8s resources in the file by your own namespace that uniquely identifies your lab group.

Add your changes, commit them and push them to the Git remote.

```
git add --all
git commit --message "<some-message> [CI SKIP]"
git push
```

## Exercise: View your artifacts in the K8s cluster

Connect to the frontend-k8s Labs cluster bastion host (get the full command from your instructor):

```
export VAULT_ADDR=<vault-address>
vault login
<vault-token>
vault ssh -mode=ca -mount-point=<ssh-secret-name> -role=<role-name> <linux-username>@<bastion-hostname>
```

Use the `kubectl get pods -w` command to monitor the pods being created in your namespace.

### Hints

```
kubectl get pods -n <namespace> -w
```

### Quiz

* What states do the pods go through?
* Why was there a delay for the changes to be applied?

## Summary

In this lab you learned how to deploy your artifacts the GitOps way using Flux.