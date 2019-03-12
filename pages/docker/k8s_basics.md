---
title: Kubernetes Basics
tags: [docker]
keywords: Kubernetes docker
last_updated: Feb 19, 2019
summary: "Basic commands and operations of Kubernetes"
sidebar: mydoc_sidebar
permalink: k8s_basics.html
folder: docker
---

## Hello World of K8s
* Start Minikube
```bash
minikube start
```

* Deploy a sample Kubernetes "deployment" to minikube
```bash
kubectl run hello-minikube --image=gcr.io/google._containers/echoserver:1.4 --port=8080
```

* Expose deployment to an external network (a.k.a create service)
```bash
kubectl expose deployment hello-minikube --type=NodePort
```

* Access the service
```bash
curl $(minikube service hello-minikube --url)
```

* Delete deployment
```bash
kubectl delete deployment hello-minikube
```

* Stop minikube
```bash
minikube stop
```

## Create K8s deployment
* create deployment YAML file
* kubectl apply -f /path/to/deployment.yaml
* minikube service deployment-name --url

## Concepts
* **Deployment** - high level representation of an application.
* **Pods** - are instances of a containerin a deployment.
* **Services** - are endpoints that export ports to outside world.

## kubectl commands
```bash
# get pods of a namespace, if namespace is not specified, display the "default" namespace
# this get command can be used with any resource type
kubectl get pods --namespace some-ns

# show the details of a pod, this command can be used with any resource type
kubectl describe pod some-pod-name 

# forward one or more local port to remote pod's port
kubectl port-forward pod-name [LOCAL_PORT:]REMOTE_PORT]

# attach to a docker container running inside a pod
kubectl attach pod-name -c container-name -n ns

# execute a command inside docker container running inside a pod
kubectl exec -it pod-name -c container-name -n ns bash

# label a resource, this command works for all resource types
kubectl label [--override] pods pod-name KEY=value

# run a particular image as a deployment on k8s cluster
kubectl run deployment-name --image=some-image:tag --port=1234
```

## Architecture
![](images/Kubernetes-architecture.png)
* Node - is a VM such as EC2. A node can run one or more pods.
* Pod - running inside node, which contains one or more docker containers.
* kubelet - monitoring the processes (pods) running on a node, like a supervisor of the node.
    It is given a set of pods to run and make sure they are running.
* kube-proxy - it glues all the nodes in a cluster together.

## Resources
* [kubectl installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* [minikube installation](https://kubernetes.io/docs/tasks/tools/install-minikube/)
* [kubectl command reference](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands)
* [kubectl cheat sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
