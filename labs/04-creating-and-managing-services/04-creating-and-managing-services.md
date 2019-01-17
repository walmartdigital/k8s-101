# Creating and Managing Services

Services provide stable endpoints for Pods based on a set of labels.

In this lab you will create the `monolith` service and "expose" the `secure-monolith` Pod externally. You will learn how to:

* Create a service
* Use label selectors to expose a limited set of Pods externally

## Tutorial: Create a Service

Explore the monolith service configuration file:

```
cat monolith-svc.yaml 
```

Create the monolith service using kubectl:

```
kubectl create -f monolith-svc.yaml
```
There are several types of services in K8s including:

* ClusterIP: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster. This is the default ServiceType.
* NodePort: Exposes the service on each Node’s IP at a static port (the NodePort). A ClusterIP service, to which the NodePort service will route, is automatically created. You’ll be able to contact the NodePort service, from outside the cluster, by requesting <NodeIP>:<NodePort>.

## Exercise: Interact with the Monolith Service

Describe the service:

```
kubectl describe services monolith
```

### Quiz

* What is the NodePort of the service?

Get the externally-accessible IP address of the service and connect to it 

```
minikube service monolith --url
```
```
curl -v --cacert ca.pem https://<NODE-IP>:<NODE-PORT>
```

### Quiz

* Why are you unable to get a response from the `monolith` service?

## Exercise: Explore the monolith Service

### Hints

```
kubectl get services monolith
```

```
kubectl describe services monolith
```

### Quiz

* How many endpoints does the `monolith` service have?
* What labels must a Pod have to be picked up by the `monolith` service?

## Tutorial: Add Labels to Pods

Currently the `monolith` service does not have any endpoints. One way to troubleshoot an issue like this is to use the `kubectl get pods` command with a label query.

```
kubectl get pods -l "app=monolith"
```

```
kubectl get pods -l "app=monolith,secure=enabled"
```

> Notice this label query does not print any results

Use the `kubectl label` command to add the missing `secure=enabled` label to the `secure-monolith` Pod.

```
kubectl label pods secure-monolith 'secure=enabled'
```

View the list of endpoints on the `monolith` service:

```
kubectl describe services monolith
```

### Quiz

* How many endpoints does the `monolith` service have?

## Exercise: Interact with the Monolith Service

Connect to the monolith service:

```
curl -v https://<NODE-IP>:<NODE-PORT>
```

### Hints

If you get an error related to the SSL certificate not matching the name of the host you're trying to connect to, edit your hosts file and make the ```local.example.com``` host name point to the IP address of the service.

```
sudo vim /etc/hosts
```

## Tutorial: Remove Labels from Pods

In this exercise you will observe what happens when a required label is removed from a Pod.

Use the `kubectl label` command to remove the `secure` label from the `secure-monolith` Pod.

```
kubectl label pods secure-monolith secure-
```

View the list of endpoints on the `monolith` service:

```
kubectl describe services monolith
```

### Quiz

* How many endpoints does the `monolith` service have?

## Summary

In this lab you learned how to expose Pods using services and labels.




Continue to [Creating and Managing Deployments](https://github.com/walmartdigital/k8s-101/blob/master/labs/05-creating-and-managing-deployments/creating-and-managing-deployments.md)