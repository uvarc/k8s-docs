# Secrets in Kubernetes

## About Secrets

The normal operation of containers often requires the injection of sensitive information
into a container, or to provide security context for a deployment such as TLS/SSL encryption
or SSH key for access to private repositories or container images.

Secrets can be consumed within pods as either `ENV` variables, or as a file at a given path.
Normal non-sensitive `ENV` variables needed by the pod can be injected as unencrypted vars,
or injected into the pod using a config map.

UVARC uses "sealed secrets" in deployments, which are encrypted and committed to source control.

Note that secrets must be scoped to a namespace, which should be a common namespace to the pod
needing to consume the secret.

## Use Cases for Secrets

Some common use cases for secrets in UVARC deployments:

- AWS keys
- API token/credentials
- Database credentials
- Private Git repository access token/PAT
- Private Registry access token/PAT
- TLS/SSL encryption certificates

## Steps for users to create Sealedsecrets for Projects.

* Install `kubeseal`
* Use the public cert in this directory `ssz-k8-sealed-secret-cert.pem`
* Create a secret with `data` elements (keys-values). Values should be a base64 version of the actual secret data. See `secret.yaml` for an example, which should look like this:

```apiVersion: v1
kind: Secret
metadata:
  creationTimestamp: null
  name: MYSECRET
  namespace: MYNAMESPACE
data:
  # The value of this key is a base64 of the "real" value.
  AWS_DEFAULT_REGION: "dXMtZWFzdC0xCg=="
  # - - -
  # *NEVER* COMMIT THIS FILE TO SOURCE CONTROL
  # - - -
```

* Once the secret is created run command against the secret eg `secret.yaml` and public certificate. 
```
cat secret.yaml | kubeseal \ 
    --controller-name=sealed-secrets \
    --controller-namespace=kube-system \
    --format yaml --cert ssz-k8-sealed-secret-cert.pem > sealedsecret.yaml
```
* The sealed secret can then be added, committed, and pushed to GitHub, where ArgoCD will create and store the secret. 
