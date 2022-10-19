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

```
apiVersion: v1
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
After being sealed, the above "raw" secret would become:
```
apiVersion: bitnami.com/v1alpha1
kind: SealedSecret
metadata:
  creationTimestamp: null
  name: MYSECRET
  namespace: MYNAMESPACE
spec:
  encryptedData:
    AWS_DEFAULT_REGION: AgC8wn1FHpZ3yhvtshzfBmdi3CDVeron33r3fdMpsibTKCQVPJb8QGF5dpaRdmP4711MDW56aKIxT2axILg/y1+7+UHFTaqb2PQYke66xd4EI+XXy8c0tFYmBA7V6H5G6rSJ3Sx7Q9vZxQf1TKVrBd4/yQTr+Im/OoXnPe22kfLF80G13eO7Ws4JyXmJmnvzoKgUw0p44CP3RccoQ3gP8FOUHmshPo8hgtAfSXsCddKG2bCdSZ5xM0M7NbceN88AfPDElUEfVMWcngMicphjSB4ifHx8d5md8HFxIw/ZT6xWQD1M46eDntvmNVurTD5IZlWRM/Mm1luJe2fi8P7K43xWyNaKN7kPEXvoSuePRTEIWeoRQlDd/Nclce/k9dqCHcSQDgCijZQQc31xKBvzljTmP0mMjQok7J98+NQW6kfV5AeXZAPWTJeU9tEuswXUvfHOmMUEMrfwDzhKS+XG75NpcwSrUDVfd7URP8dl+CybOfoMFYL2vtAOhmMP5WzON1J+CFXg9B88NKoNweuT6AYgRjdcfybNXS9Q7SRQQ9q41gGXIPsMdvIlRWu3tq4TcHdKRuWHzrwDrUYwZkM+pirrwiPVSWQ5USSLtSb4i5XFAP4yC2+caMwE9/1rD4NQT6ZyzybzmWWP0Hxrn/WZ5xPagNnVwdU9esLyQnYbcrpUkqTuM+6vuzcV4lY/FSZHz6WVW6RcNN4J2Z4q
  template:
    metadata:
      creationTimestamp: null
      name: MYSECRET
      namespace: MYNAMESPACE

```

* The sealed secret can then be added, committed, and pushed to GitHub, where ArgoCD will create and store the secret. 
