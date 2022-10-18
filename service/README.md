# Deploy a Kubernetes Service

The enclosed YAML snippets define a wide variety of service options. Not all are required.
File names follow no strict convention beyond convenience of the developer. As you will observe,
the unique functionality of each snippet is based upon the "kind" parameter. These snippets
could also be concatenated into a single file.

Required:

- The "deployment" spec is required as it defines the container pod and its basic parameters.
- The "service" spec is required if the pod functions as a network service. It serves as an abstract way to expose an application running on a set of Pods as a network service.
- The "ingress" spec is required if you want to map external traffic into your pod via the ingress controller. This can also handle SSL for your site.

Optional:

- The "config-map" spec is an optional mechanism of passing KV environment variables into your pod.
- The "sealed secret" spec is a way of passing encrypted data (passwords, tokens, keys, etc.) into your pod as ENV vars.
- The "volume claim" spec is a way of consuming an existing Persistent Volume that maps to storage.

Files:

- [config-map.yaml](config-map.yaml)    
- [deployment.yaml](deployment.yaml)
- [ingress.yaml](ingress.yaml)
- [sealed-secret.yaml](sealed-secret.yaml)
- [secret.yaml](secret.yaml)
- [service.yaml](service.yaml)
- [volume-claim.yaml](volume-claim.yaml)

