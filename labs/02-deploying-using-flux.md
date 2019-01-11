# Deploying using Flux

Flux is a tool that automatically ensures that the state of a cluster matches the config in git. It uses an operator in the cluster to trigger deployments inside Kubernetes, which means you don't need a separate CD tool. It monitors all relevant image repositories, detects new images, triggers deployments and updates the desired running configuration based on that (and a configurable policy).

Flux is our tool for implementing GitOps.

In this lab you will learn how to:

* Deploy an artifact using Flux
* Trigger Flux to force an update to an artifact
* Configure your deployment so that Flux deploys your artifacts conditionally

## Exercise: Deploy using Flux

On your local machine, clone the deployment repository for the labs cluster (ask your instructor for the URL).

```
git clone https://<git-remote>/walmartdigital/frontend-k8s
```

Copy the `src/k8s/02/monolith.yaml` file to the "deployments" folder, making sure to rename it so that it has a unique name which easily identifies your team. Don't forget to update the namespace in `monolith` pod declaration.

```
cp <path-to-k8s-101-repo>/src/k8s/02/monolith.yaml <path-to-deployment-repo>/frontend-k8s/deployments/labs/eastus2/alice-and-bob.yaml
```

Commit the file and push it to the Git remote.

Use the `kubectl get` command to monitor the pods being created in your namespace.

### Hints

```
kubectl get pods -n <namespace> -w
```

### Quiz

* What are the different states that the `monolith` pod goes through?
* Why was there a delay for the changes to be applied?

## Exercise: Make changes and trigger Flux to sync

Add the contents of the `src/k8s/02/command-demo.yaml` file at the end of your deployment manifest.

```
apiVersion: v1
kind: Namespace
metadata:
  name: group-name
---
apiVersion: v1
kind: Pod
metadata:
  name: monolith
...
---
apiVersion: v1
kind: Pod
metadata:
  name: command-demo
  namespace: group-name
  labels:
    purpose: demonstrate-command
spec:
  containers:
  - name: command-demo-container
    image: debian
    command: ["printenv"]
    args: ["HOSTNAME", "KUBERNETES_PORT"]
  restartPolicy: OnFailure
```

Use the `kubectl get` command to monitor the new pod being created in your namespace.

In another terminal, use fluxctl to forcefully trigger Flux to sync with deployment repository.

### Hints

```
fluxctl sync --k8s-fwd-ns flux
```

### Quiz

* What is the output of the fluxctl utility?