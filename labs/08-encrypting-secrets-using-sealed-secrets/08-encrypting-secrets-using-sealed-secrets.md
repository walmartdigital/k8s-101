# Protecting Secrets using Sealed Secrets

In this part of the lab, we will revisit the concept of Kubernetes `secrets`. We will look at some of their limitations and ways to get around these limitations.

## Exercise: Decoding K8s Secrets

On your local machine (in minikube), get the `tls-certs` secret that was created in step 3 of this lab.

```
kubectl get secret tls-certs -o yaml
```

### Quiz

* What are the keys included in the `tls-certs` secret?
* What are the elements of the `tls-certs` secret that should be considered to be sensitive information?
* What is the value of the `TYPE` field of the secret?

Copy the value of the `key.pem` key of the `tls-certs` secret and run the following command:

```
echo "<value-of-key.pem>" | base64 -D
```

or

```
pbpaste | base64 -D
```

### Quiz

* What is the output of the `base64` utility?
* What does that tell us about how Kubernetes secrets are formatted?
* What type of problems would arise if we were to mantain such secrets in a Git repository (as part of a CI/CD strategy)?

## Exercise: Using Kubeseal to Protect Kubernetes Secrets

As discussed in the previous step, Kubernetes secrets cannot be stored securely. In this part of the lab, we will encrypt our secret into a `SealedSecret` [https://github.com/bitnami-labs/sealed-secrets], which is safe to store - even to a public repository. The SealedSecret can be decrypted only by the controller running in the target cluster and nobody else (not even the original author) is able to obtain the original Secret from the SealedSecret.

Generate the YAML manifest for the secret, making sure to specify a namespace that uniquely identifies your lab team (the same that you used in lab 06):

```
kubectl create secret generic tls-certs --from-file=tls/ -n <namespace> -o yaml --dry-run > tls-certs-secret.yaml
```

Check that the secret was properly created.
```
cat tls-certs-secret.yaml
```

Use the `kubeseal` utility to encrypt the secret:

```
kubeseal --cert sealed-secrets.cert --format=yaml < tls-certs-secret.yaml > sealed_tls-certs-secrets.yaml
```

Check that the SealedSecret was properly created.
```
cat sealed_tls-certs-secrets.yaml
```

Open the `sealed_tls-certs-secrets.yaml` file and try to base64-decode the value of the `key.pem` key.

### Hints

Copy the value of the `key.pem` key of the `tls-certs` secret and run the following command:

```
echo "<value-of-key.pem>" | base64 -D
```

or

```
pbpaste | base64 -D
```

### Quiz

* What is the `kind` of K8s resource that was created by the `kubeseal` utility?

## Exercise: Deploy the SealedSecret in the Target Cluster

In this part of the lab, we will deploy the SealedSecret in a cluster that has the private key that corresponds to the certificate that we used to encrypt the SealedSecret.

Copy the contents of the `sealed_tls-certs-secrets.yaml` and add it to your manifest file in the deployment repository corresponding to the backend Labs cluster.

Add, commit and push the changes to the remote Git repository.

Connect to the backend-k8s Labs cluster bastion host (get the full command from your instructor):

```
export VAULT_ADDR=<vault-address>
export VAULT_TOKEN=<vault-token>
vault ssh -mode=ca -mount-point=<ssh-secret-name> -role=<role-name> <linux-username>@<bastion-hostname>
```

List the SealedSecrets and secrets that are present in your namespace.

```
kubectl get sealedsecrets -n <namespace>
kubectl get secrets -n <namespace>
```

Extract the value of the `key.pem` field of the `tls-certs` secret and decode it.

### Hints

```
kubectl get secrets tls-certs -n <namespace> --output="jsonpath={.data.key\.pem}" | base64 -d
```

### Quiz

* What SealedSecrets are present in the namespace?
* What secrets are present in the namespace?
* What is the value of the `key.pem` field of the `tls-certs` secret?

## Summary

In this lab you learned how to protect Kubernetes secrets using SealedSecret so that they can be stored in a Git repository.

