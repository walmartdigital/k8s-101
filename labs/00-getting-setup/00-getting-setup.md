# Basic Setup
The lab is divided in two parts. One that uses Minikube, a tool that runs a single-node Kubernetes cluster in a virtual machine on your laptop. The second part will have you deploy artifacts in a remote Kubernetes cluster.

The exercises in this lab will have you use a variety of tools that you should install before attending the workshop. This guide attempts to provide instructions on how to install these tools for MacOs and Homebrew users.

## Prerequisites
Before attempting to clone repo, be sure your host machine meets the prerequisites:

* git
* [Homebrew](https://brew.sh)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - direct download link [here](https://download.virtualbox.org/virtualbox/6.1.2/VirtualBox-6.1.2-135662-OSX.dmg)
* [Minikube](https://kubernetes.io/docs/tasks/tools/install-minikube/#before-you-begin)
* [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* curl/wget
* [Kubeseal](https://github.com/bitnami-labs/sealed-secrets)
* [Vault](https://www.vaultproject.io/)
  
## Install VirtualBox
You can download from VirtualBox.org with this [link](https://download.virtualbox.org/virtualbox/6.1.2/VirtualBox-6.1.2-135662-OSX.dmg) or using homebrew formula.
```
    brew cask install virtualbox
```

## Install minikube
Run the installation command:
```
    brew install minikube
```

Check your version.
```
    minikube version
```

## Install kubectl
Use the Kubernetes command-line tool, kubectl, to deploy and manage applications on Kubernetes. Using kubectl, you can inspect cluster resources; create, delete, and update components; look at your new cluster; and bring up example apps.

```
    brew install kubernetes-cli
```

Test to ensure the version you installed is sufficiently up-to-date:
```
    kubectl version
    Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.2", GitCommit:"59603c6e503c87169aea6106f57b9f242f64df89", GitTreeState:"clean", BuildDate:"2020-01-23T14:21:54Z", GoVersion:"go1.13.6", Compiler:"gc", Platform:"darwin/amd64"}
    Server Version: version.Info{Major:"1", Minor:"16", GitVersion:"v1.16.7", GitCommit:"be3d344ed06bff7a4fc60656200a93c74f31f9a4", GitTreeState:"clean", BuildDate:"2020-02-11T19:24:46Z", GoVersion:"go1.13.6", Compiler:"gc", Platform:"linux/amd64"}
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

## Install Vault with brew
```
brew install vault
```

## or  Vault binary
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
minikube start --memory 4096 --cpus 4 --kubernetes-version v1.16.7
...
😄  minikube v1.7.2 on Darwin 10.15.3
✨  Automatically selected the hyperkit driver
🔥  Creating hyperkit VM (CPUs=4, Memory=4096MB, Disk=20000MB) ...
🐳  Preparing Kubernetes v1.16.7 on Docker 19.03.5 ...
💾  Downloading kubelet v1.16.7
💾  Downloading kubectl v1.16.7
💾  Downloading kubeadm v1.16.7
🚀  Launching Kubernetes ...
🌟  Enabling addons: default-storageclass, storage-provisioner
⌛  Waiting for cluster to come online ...
🏄  Done! kubectl is now configured to use "minikube"
```

### Run hello-minikube 
```
kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node
```

### Expose deploymnet

```
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

### Make sure the `STATUS` of the hello-node-... pod is `RUNNING`
```
kubectl get po -w
```

### Test the url.
```
minikube service hello-node

OR

curl -v $(minikube service hello-node --url)
  
* Trying 192.168.64.4...
* TCP_NODELAY set
* Connected to 192.168.64.4 (192.168.64.4) port 31124 (#0)
> GET / HTTP/1.1
> Host: 192.168.64.4:31124
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 200 OK
< Date: Fri, 14 Feb 2020 02:08:36 GMT
< Connection: keep-alive
< Transfer-Encoding: chunked
<
* Connection #0 to host 192.168.64.4 left intact
Hello World!* Closing connection 0
```

Now you can go to [Creating and managing pods](https://github.com/walmartdigital/k8s-101/blob/master/labs/01-creating-and-managing-pods/01-creating-and-managing-pods.md)



UnderWorld Team with :heart: