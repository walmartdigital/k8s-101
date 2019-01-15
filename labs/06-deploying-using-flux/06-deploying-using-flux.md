# Deploying using Flux and exposing your artifacts to the Internet

In this lab, we will deploy our microservices in our Cloud.

Flux is a tool that automatically ensures that the state of a cluster matches the config in git. It uses an operator in the cluster to trigger deployments inside Kubernetes, which means you don't need a separate CD tool. It monitors all relevant image repositories, detects new images, triggers deployments and updates the desired running configuration based on that (and a configurable policy).

Flux is our tool for implementing GitOps in the Cloud.

In this lab you will learn how to:

* Use namespaces to isolate your artifacts from other users of the cluster
* Deploy an artifact using Flux
* Trigger Flux to force an update to an artifact
* Use ingress rules to expose your artifacts to the Internet

# Using namespaces 

Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces. Namespaces are a way to divide cluster resources between multiple users.

## Exercise: Create your own namespace

In the `hello-app.yaml` file, replace all occurences of `group-name` by a value that uniquely identifies your lab group in order to namespace your resources. Note that you will have to use the same namespace for all resources you create throughout this workshop.

```
vim hello-app.yaml
```

## Exercise: Deploy using Flux

On your local machine, clone the deployment repository for the labs cluster (ask your instructor for the URL).

```
git clone https://<git-remote>/walmartdigital/frontend-k8s
```

Copy the `hello-app.yaml` file to the "deployments" folder, making sure to rename it so that it has a unique name which easily identifies your team, e.g., `bob-and-alice.yaml`.

```
cp <path-to-k8s-101-repo>/06-deploying-using-flux/hello-app.yaml <path-to-deployment-repo>/frontend-k8s/deployments/labs/eastus2/alice-and-bob.yaml
```

Add your changes, commit them and push them to the Git remote.

```
git add --all
git commit --message "<some-message>"
git push
```

## Exercise: View your artifacts in the K8s cluster

Connect to the frontend-k8s Labs cluster bastion host:

```
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

# Using Ingress Rules

Ingress rules expose HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the ingress resource.

    internet
        |
  [ Ingress ]
  --|-----|--
  [ Services ]

An ingress can be configured to give services externally-reachable URLs, load balance traffic, terminate SSL, and offer name-based virtual hosting. An ingress controller is responsible for fulfilling the ingress, usually with a loadbalancer, though it may also configure your edge router or additional frontends to help handle the traffic.

## Exercise: Expose your Artifact to the Internet using an Ingress Rule

Edit the `ingress.yaml` file and perform the following changes:
* Replace the namespace with the one used in the rest of your manifest file
* Replace the hostname in the ingress rule with one that uniquely identifies your group, e.g., `bob-and-alice.labs-rneaxc.walmartdigital.cl`

Add the contents of the edited `ingress.yaml` file at the end of your K8s manifest and commit your changes to the remote deployment repository.

Use the `kubectl get ingress -n <namespace> -w` command to monitor the new resource being created in your namespace.

Once the ingress resource is created, use the `kubectl describe ingress hello -n <namespace>` command to inspect the Ingress object.

## Exercise: Interact with your Artifact from the Internet

Open a browser and navigate to the URL specified in the ingress rule host field, e.g., https://bob-and-alice.labs-rneaxc.walmartdigital.cl.

Click on the lock icon in the address bar and click to view the certificate presented by the website.

### Quiz

* When is the expiration date of the certificate?
* What is the common name (CN) that appears in the certificate?
* What is issuing authority of the certificate?

## Summary

In this lab you learned how to deploy your artifacts the GitOps way using Flux and how to expose your services to the Internet.