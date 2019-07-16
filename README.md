# K8S (and related services) on GCP

These recepies are for Google's Deployment Manager.
Whole infrastructure is defined in relevant .yaml files.

Kubernetes cluster config file automatically deploys Weave Flux, which then takes over apps deployment.

In that way, a complete cluster can be recreated from a set of versioned and audietd config files.

Names and relevant values (project, region, private network IP address etc) are currently hardcoded.

TODO 
- define VPC through config

Prerequisites:
- Container Registry

Notes: 
- if you use third-party container registry (i.e. not the one from Google), you'd need to setup appropriate credentials for Weave Flux. Follow their guide for this.
- It is a BAD idea to store a git key into git itself :) You'd need a different way (maybe via GCP's Cloud KMS?) to supply the git key, which Flux needs in order to pull and deploy your k8s resources.
- I'm still experimenting with granting the k8s' service-account appropriate permissions (or rather, appropriate entry into bucket's ACL) for access to GCP container registry, for full automation. Check this Stackoverflow thread for issues: https://stackoverflow.com/questions/56759231/gcp-grant-a-service-account-permission-to-write-in-a-gcs-bucket-with-deployment_


## Deployment
```java
gcloud deployment-manager deployments create i2software-kubernetes --template gcp/k8s/cluster.jinja

gcloud deployment-manager deployments update i2software-kubernetes --template gcp/k8s/cluster.jinja

gcloud deployment-manager deployments delete i2software-kubernetes

gcloud deployment-manager deployments delete i2software-cloudsql --delete-policy=ABANDON


```
