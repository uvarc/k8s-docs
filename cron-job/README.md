# Kubernetes Cron Jobs

Like regular CRON jobs in Linux systems, a k8s-based cron task runs on a schedule.
Cron Jobs can consume ENV vars, volume claims, secrets like other deployments.

Note:

- A NAMESPACE is required, as are the RESOURCE definitions.
- The `ttlSecondsAfterFinished` parameter allows the developer to retain logs for this duration (in seconds).
- Time for the UVARC K8S cluster is UTC. Schedule accordingly.
