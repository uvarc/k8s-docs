# k8s-docs

<img src="https://kubernetes.io/images/kubernetes-horizontal-color.png" style="float:right;width:400px;" alt="Kubernetes" />

UVARC documentation and sample templates for K8S deployments.

## Deployment/Config Spec Files

The attached documentation covers the syntax and formatting for these typical deployments:

- [**Services**](services/) - long-running, network-exposed pods such as web servers and APIs.
- [**Secrets**](secrets/) - A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key.
- [**Jobs**](jobs/) - short-lived, single-run pods such as a task or execution.
- [**Cron Jobs**](cron-jobs/) - recurring jobs set on a schedule.

The Service spec files explain the most about other available options for your pod, such as:

- Persistent Volume Claims (storage) needed to provide data or storage to your application.
- Secrets (encrypted keys, tokens, passwords, etc.) needed to run your application.
- Config Maps (ENV vars) needed to run your application in a given environment.
- Ingress to map your service to a domain name, with/without HTTPS.

## Getting Started

If you would like to manage your own deployment(s), do the following:

1. Contact the RC DevOps team <uvarc-devops@virignia.edu> to create a NAMESPACE and `kube/config` file for you. This file will contain read-only credentials you will use below. The `.kube/config` file generated for you should be incorporated into any local `.kube/config` file you have for other environments (if any).
2. Create a clean, empty Git repository and [copy these files](https://s3.amazonaws.com/uvarc-k8s/stubs/deployment-bundle.tar.gz) into it.
3. Within the `templates/` directory, create a new folder for each deployment. Populate that folder with the necessary files (with updated values for your deployment, service, namespace, etc.) from this documentation.
4. Work with the DevOps team to connect your Git repo with ArgoCD, our deployment management tool.

## Observing Your Deployments

Download [**Lens**](https://k8slens.dev/), a Kubernetes GUI, and connect to our cluster (over UVA Anywhere or from Grounds). Using the Namespace selector 
in the upper-right, choose your namespace and you will have observatility to any of your pods. From there you can view logs, shell into pods, and other 
functions.

NOTE: Lens serves as a read-only tool. If you want to start, stop, or change a deployment, that must be done through your deployment code, where ArgoCD will ensure the state of your application conforms to its defined state in code.
