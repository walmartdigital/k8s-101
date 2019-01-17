# Exposing Frontends and Backends

Ingress rules expose HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the ingress resource.

    internet
        |
  [ Ingress ]
  --|-----|--
  [ Services ]

An ingress can be configured to give services externally-reachable URLs, load balance traffic, terminate SSL, and offer name-based virtual hosting. An ingress controller is responsible for fulfilling the ingress, usually with a loadbalancer, though it may also configure your edge router or additional frontends to help handle the traffic.

## Exercise: Expose a Frontend to the Internet using an Ingress Rule

In this part of the lab, we will expose a frontend service to the Internet via the Traefik reverse proxy. Traefik is a modern Cloud native HTTP reverse proxy and load balancer that makes deploying and managing frontend applications easy and scalable.

Edit the `ingress-front.yaml` file and perform the following changes:
* Replace the namespace with the one used in the rest of your manifest file
* Replace the hostname in the ingress rule with one that uniquely identifies your group, e.g., `bob-and-alice.labs-rneaxc.walmartdigital.cl`

Add the contents of the edited `ingress-front.yaml` file at the end of your K8s manifest and commit your changes to the remote deployment repository.

Use the `kubectl get ingress -n <namespace> -w` command to monitor the new resource being created in your namespace.

Once the ingress resource is created, use the `kubectl describe ingress hello -n <namespace>` command to inspect the Ingress object.

## Exercise: Interact with the Frontend from the Internet

Open a browser and navigate to the URL specified in the ingress rule host field, e.g., https://bob-and-alice.labs-rneaxc.walmartdigital.cl.

Click on the lock icon in the address bar and click to view the certificate presented by the website.

### Quiz

* When is the expiration date of the certificate?
* What is the common name (CN) that appears in the certificate?
* What is issuing authority of the certificate?

## Exercise: Expose a Backend to the Internet using an Ingress Rule

In this part of the lab, we will expose a backend service to the Internet via the Kong API gateway. Kong is a cloud-native, fast, scalable, and distributed Microservice Abstraction Layer (also known as an API Gateway). Its an open source projet and its core values are high performance and extensibility.

On your local machine, clone the deployment repository for the backend labs cluster (ask your instructor for the URL).

```
git clone https://<git-remote>/walmartdigital/backend-k8s
```

Copy the `hello-app.yaml` file to the "deployments" folder, making sure to rename it to a name that uniquely identifies your team, e.g., `bob-and-alice.yaml`.

```
cp <path-to-k8s-101-repo>/06-deploying-using-flux/hello-app.yaml <path-to-deployment-repo>/backend-k8s/deployments/labs/eastus2/alice-and-bob.yaml
```

Replace the namespace of all K8s resources in the file with a value that uniquely identifies your lab group.

Add the contents of the `ingress-back.yaml` file to the end of your manifest file, making sure to perform the following changes:
* Replace the `group-name` namespace value by one that uniquely identifies your lab team
* Replace the `path` field of the Ingress object to one that uniquely identifies your artifacts, e.g. `/hello-bob-and-alice`
* Replace the value of the `serviceName` field of the Ingress object with the name of the service you would like to expose via the ingress rule
* Replace the value of the `servicePort` field of the Ingress object with the port number at which the backend service is listening

Add you changes and commit them and push them to the remote repository.

Connect to the backend-k8s Labs cluster bastion host (get the full command from your instructor):

```
export VAULT_ADDR=<vault-address>
vault login
<vault-token>
vault ssh -mode=ca -mount-point=<ssh-secret-name> -role=<role-name> <linux-username>@<bastion-hostname>
```

Inspect the objects that were created as part of your deployment.

### Hints

```
kubectl get po -n <group-name>
kubectl describe ingress hello-back -n <group-name>
kubectl describe secret acme-crt-secret -n <group-name>
```

### Quiz

* How many replicas of the `hello` pod are running?
* At which Internet-accessible URL will the service be accessible?
* What keys are included in the `acme-crt-secret` secret?

Access the `hello` service from the Internet:

```
curl -v https://<hostname>/<path>
```

## Summary

In this lab you learned how to expose your artifacts to the outside of the Kubernetes cluster using Ingress rules.

Continue with [Encrypting Secrets Using Sealed Secrets](https://github.com/walmartdigital/k8s-101/blob/master/labs/08-encrypting-secrets-using-sealed-secrets/08-encrypting-secrets-using-sealed-secrets.md)