---
title: Kubernetes Health Check
tags: [docker]
keywords: Kubernetes health check probe
last_updated: March 19, 2019
summary: "Kubernetes has two types of health checks: readiness probes and liveness probes"
sidebar: mydoc_sidebar
permalink: k8s_health_check.html
folder: docker
---

## Readiness vs Liveness
* Readiness probe is to check whether a pod is ready to take calls from external world. 
* Liveness probe is to check whether a pod is healthy or not. This probe is after the readiness probe.

Readiness and livenes probes can be achieved from inside or outside containers.
* HTTP or TCP requests to pod
* successful command execution on pod

Probes are defined **on the container in a deployment or pod specification**.

## Example
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
        readinessProbe:
          httpGet:
            path: /
            port: 8080
          initialDelaySeconds: 15
          periodSeconds: 3
        livenessProbe:
          httpGet:
            path: /
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 30          
```

After the deployment above is applied, you can describe the deployment for details of probes by:
```bash
kubectl describe deployment DEPLOYMENT-NAME
```