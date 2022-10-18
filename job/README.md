# Kubernetes Jobs

A Job creates one or more Pods and will continue to retry execution of the Pods until a specified number of them successfully terminate. As pods 
successfully complete, the Job tracks the successful completions. When a specified number of successful completions is reached, the task (ie, Job) is 
complete. Deleting a Job will clean up the Pods it created. Suspending a Job will delete its active Pods until the Job is resumed again.

A simple case is to create one Job object in order to reliably run one Pod to completion. The Job object will start a new Pod if the first Pod fails or is 
deleted (for example due to a node hardware failure or a node reboot).

You can also use a Job to run multiple Pods in parallel.

Like Cron Jobs and Service Deployments, Jobs can use all k8s resources such as ENV variables,
secrets, volume claims, config maps, etc. in order to do their work.

Note that a NAMESPACE and RESOURCE definition is required for Jobs in our environment.
