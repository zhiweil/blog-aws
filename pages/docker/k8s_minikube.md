---
title: Minikube
tags: [docker]
keywords: Kubernetes docker minikube kubectl
last_updated: March 14, 2019
summary: "Run Kubernetes locally"
sidebar: mydoc_sidebar
permalink: k8s_minikube.html
folder: docker
---
## Installation
```bash
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube
mv minikube /usr/local/bin # or whichever location can be found in $PATH
```
## Confiure kubectl for minikube
If kubectl is not installed yet, please run the following commands.
```bash
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x kubectl
mov kubectl /usr/local/bin # or whatever location can be found in $PATH
```
## Configuration
```bash
# Start minikube first
minikube start -p PROFILE-NAME # PROFILE-NAME will be a new context

# you can find kubectl's config file at $HOME
cat ~/.kube/config

# configure kubectl to interact with a k8s cluster. You can see all the available contexts in ~/.kube/config.
kubectl config use-context CONTEXT-NAME
```

## Dashboard
The following command will configure and launch a dashboard web page.
```bash
minikube dashboard
```

## Resources
* [Minikube's Github page](https://github.com/kubernetes/minikube)
