---
title: Label and Selector
tags: [docker]
keywords: Kubernetes Label Selector
last_updated: March 17, 2019
summary: "Kubernetes label is an method to keep its components organized. Labels can also be used by k8s to identify resources to act on."
sidebar: mydoc_sidebar
permalink: k8s_label_and_selector.html
folder: docker
---
## Label
* Labels are key-value pairs, which you can attach to all kinds of kubernetes components such as services, pods and etc.
* Labels do not affect kubernetes semantics. 

## Selector
Selectors are a way of expressing what objects to select based on their labels. 

By selectors you can specify if a label equals a criteria or inside a set of criteria.
* equality-based
* set-based

You can label all kinds of components in kubernetes such as 
* deployment
* service
* node

## nodeSelector
Label nodes and create nodeSelector on deployment, so that only the nodes with specific label will be chosen to deploy pods.

```bash
# Get a list of nodes
kubectl get nodes

# label a specific node
kubectl label node NODE-NAME labelName=labelValue

# this provices the details of a node includind labels of the node
kubectl describe node NODE-NAME
```

In deployment YAML, nodeSelector is specified.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: my-deployment
spec:
    selector:
      matchLabels:
        app: tomcat
    replicas: 2
    template:
      metadata:
        labels:
          app: tomcat
      spec:
        containers:
          - name: tomcat
            image: tomcat:9.0
            ports: 
            - containerPort: 8080
        nodeSelector:
          labelName: labelValue
```

