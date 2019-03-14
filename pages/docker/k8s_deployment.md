---
title: Deployment
tags: [docker]
keywords: Kubernetes docker deployment
last_updated: March 14, 2019
summary: "Deployment is a high level object of kubernetes. It defines the desired state of an application."
sidebar: mydoc_sidebar
permalink: k8s_deployment.html
folder: docker
---
## Architecture
Kubernetes consists of the following two objects:
* **Pods**
* Optionally, **ReplicaSets** that are automatically managed by Deployment.

## Useful commands
```bash
# list deployments
kubectl get deployments

# view status of deployment roll outs
kubectl rollout status deployment DEPLOYMENT-NAME

# set the image of deployment
kubectl set image deployment/DEPLOYMENT-NAME CONTAINER-NAME=DOCKER-IMAGE:TAG

# view the history of roll outs, including previous revisions
kubectl rollout history deployment/DEPLOYMENT-NAME 
# the previous command will give you revision number of eash history entry. By the following
# comamnd you can view the details of each revision
kubectl rollout history deployment/DEPLOYMENT-NAME --revision=4
```

