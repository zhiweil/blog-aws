---
title: Kubernetes Networking
tags: [docker]
keywords: Kubernetes networking
last_updated: March 20, 2019
summary: "Kubernetes networking basics"
sidebar: mydoc_sidebar
permalink: k8s_networking.html
folder: docker
---
## Concepts
* **Cluster** - is a foundation of kubernetes engine. It has various components at different levels such as master node, worker nodes, 
    cluster API, deployment, service, pod, pod replica, docker container and etc.
* **Node** - nodes are VMs including master node and worker nodes.
* **Pod** - pods run on worker nodes, it contains one or more docker containers. 
* **Deployment** - pod is defined in deployment. A deployment can have one or more pods.
* **Service** - deployment can expose one or more services.

## Fundamental Requirements
* All containers can communicate with all other containers without NAT.
* All nodes can communicate with all containers without NAT.
* The IP that a container sees itself is the same as the others see it.

The networking of kubernetes are implemented at the following three levels:
* Pod and Container
* Service
* Ingress network

## Pod and container
A pod conists of one or more containers that are allocated on the same host (node), and are configured to 
share a network stack and other other resources such as volumes.
* On host machine (node), it has a docker bridge network interface Docker0 (i.e. with IP 127.17.0.1/24).
* All the containers running on the node are put on the same LAN as Docker0, so they will have IP like 172.17.0.x/24.
* The host machine's phsical NI will be allocated an IP such as 10.0.0.1/16, which can communicate with Docker0. 

## Resources
<iframe width="560" height="315" src="https://www.youtube.com/embed/OaXWwBLqugk" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>