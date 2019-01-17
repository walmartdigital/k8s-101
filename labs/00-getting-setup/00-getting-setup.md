# Basic Setup
The whole lab should be based on minikube, a tool that runs a single-node Kubernetes cluster in a virtual machine on your laptop.
This guide is based on MacOs and Homebrew installation.

## Prerequisites
Before attempting to clone repo, be sure your host machine meets the prerequisites:

* git
* [Homebrew](https://brew.sh)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - direct download link [here](https://download.virtualbox.org/virtualbox/6.0.2/VirtualBox-6.0.2-128162-OSX.dmg)
* [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/#before-you-begin)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* curl/wget
* [Kubeseal](https://github.com/bitnami-labs/sealed-secrets)
* [Vault](https://www.vaultproject.io/)
  
## Install VirtualBox
You can download from VirtualBox.org with this [link](https://download.virtualbox.org/virtualbox/6.0.2/VirtualBox-6.0.2-128162-OSX.dmg) or using homebrew formula.
```
    brew cask install virtualbox
```

## Install minikube
Run the installation command:
```
    brew cask install minikube
```

Check your version.
```
    minikube version
    minikube version: v0.32.0
```

## Install kubectl
Use the Kubernetes command-line tool, kubectl, to deploy and manage applications on Kubernetes. Using kubectl, you can inspect cluster resources; create, delete, and update components; look at your new cluster; and bring up example apps.

```
    brew install kubernetes-cli
```

Test to ensure the version you installed is sufficiently up-to-date:
```
    kubectl version
    Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.2", GitCommit:"cff46ab41ff0bb44d8584413b598ad8360ec1def", GitTreeState:"clean", BuildDate:"2019-01-13T23:15:13Z", GoVersion:"go1.11.4", Compiler:"gc", Platform:"darwin/amd64"}
```

## Install curl
```
    brew install curl
```

## Install kubeseal
Encrypt your Secret into a SealedSecret, which is safe to store.
```
    brew install kubeseal
```

## Install Vault
```
wget https://releases.hashicorp.com/vault/1.0.2/vault_1.0.2_darwin_amd64.zip
```

## Install Keybase
```
wget https://prerelease.keybase.io/Keybase.dmg
```

## Clone the project from Github
```
    git clone https://github.com/walmartdigital/k8s-101.git
```

## Quickstart

### Start Minikube

```
minikube start
...
Starting local Kubernetes v1.12.4 cluster...
Starting VM...
Getting VM IP address...
Moving files into cluster...
Setting up certs...
Connecting to cluster...
Setting up kubeconfig...
Verifying kubelet health ...
Verifying apiserver health ...Kubectl is now configured to use the cluster.
Loading cached images from config file.


Everything looks great. Please enjoy minikube!
```

### Run hello-minikube 
```
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
```

### Expose deploymnet

```
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

### Test the url.
```
curl -v $(minikube service hello-node --url)
```

Now you can go to [Creating and managing pods](https://github.com/walmartdigital/k8s-101/blob/master/labs/01-creating-and-managing-pods/01-creating-and-managing-pods.md)



UnderWorld Team with :heart: