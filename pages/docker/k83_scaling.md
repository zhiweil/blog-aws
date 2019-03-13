---
title: Kubernetes Scaling
tags: [docker]
keywords: Kubernetes docker scaling
last_updated: March 13, 2019
summary: "Scaling Kubernetes application"
sidebar: mydoc_sidebar
permalink: k8s_scaling.html
folder: docker
---
## Stateful and stateless. 
* **Stateful** - application state are persisted by application and used across different sessions. If a
    stateful application is deleted from cluster, its state get lost.
* **Stateless** - application state are persisted in datastore (such as DB). If a stateless application is 
    deleted from cluster, its state can still be loaded from DB.
Kubernetes handle stateful and stateless applications differently.

**It is recommended to create stateless applications.**

## Replica
Kubernetes allows you to define replicas when you deploy and application. The options are:
* setting replica in deployment(**recommended approach**)
* Defining a ReplicaSet
* Bare Pods
* Job
* DaemonSet

## How to create Replica
* In deployment YAML file, specify "replicas: 4" and then create deployment by kubectl apply.
   ```bash
    kubectl apply -f /path/to/deployment/yaml
    ```
* For an existing k8s deployment, use kubectl scale command.
    ```bash
    kubectl scale --replicas=4 deployment/deployment-name 
    ```
    
After k8s deployment is scaled, the following commands can be used to check the status of the updated deployment.
```bash
kubectl get deployments -o wide
kubectl describe depolyments deployment-name
```

## Load Balancing
Instead of creating a NodePort service for a single replica, multiple replicas will need to create a
LoadBalancer service by the following command.
```bash
kubectl expose deployment deployment-name --type=LoadBalancer --port=8080 --remote-port=8080 --name service-name
```
* **--port=8080** - this define the port exposed by load balancer.
* **--remote-port=8080** - this defines the port exposed by replicas.

The new LoadBalancer service details, such as the IP addresses assigned to LB, can be checked by:
```bash
kubectl describe services service-name
```