* Explain the problem with secrets in code in a Devops environment
* Define what a K8s secret is and how it is different from a ConfigMap
* Demonstrate the limitations of secrets in the sense that they are only base64 encoded
* Introduce SealedSecrets and explain its architecture and how it solves the aforementioned problem
* Explain how to locate the certificate for a given cluster/sealedsecrets controller
* Demonstrate how to encrypt a generic secret and how to encrypt a registry secret using Kubeseal
* Show how the secrets are decrypted within the cluster and how they are protected in the Git repository