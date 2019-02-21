---
title: EKS Clusters and Nodes
tags: [computing]
keywords: AWS Computing EKS Kubernetes
last_updated: Feb 20, 2019
summary: "Amazon Elastic Container Service for Kubernetes (EKS) Clusters and Nodes"
sidebar: mydoc_sidebar
permalink: eks_cluster_node.html
folder: computing
---
An EKS cluster consists of two primary components:
* EKS control plane
* EKS worker nodes registered with the control plane

## Clusters
When an Amazon EKS cluster is created, the IAM entity (user or role) that creates the cluster is
added to the Kubernetes RBAC authorization table as the administrator (with system:master
permissions. Initially, only that IAM user can make calls to the Kubernetes API server using
kubectl.

### Create
The following CLI creates a cluster. EKS cluster creation usually takes several minutes, bu the command returns immediately.

```bash
aws --region us-east-1 --profile aws-zhiwei eks create-cluster --name dev-cluster \
 --role-arn arn:aws:iam::457964311029:role/eksServiceRole \
 --resources-vpc-config subnetIds=subnet-04b02dd061f4129e4,subnet-0ac348a0978bd47ca,subnet-0be7c84985cc78393,securityGroupdIds=sg-011fb571c9fc31fcc
```

Query cluster status until it becomes "ACTIVE".
```bash
aws --region us-east-1 --profile aws-zhiwei eks describe-cluster --name dev-cluster --query cluster.status --output text
```

When cluster provisioning is done, you can retrieve its endpoint and certificationAuthority.data values wit the following command
```bash
aws --region us-east-1 --profile aws-zhiwei eks describe-cluster --name dev-cluster --query cluster.endpoint --output text
aws --region us-east-1 --profile aws-zhiwei eks describe-cluster --name dev-cluster --query cluster.certificateAuthority.data --output text
```

### Update
When a new K8s version is available, you can update your cluster. **You should test your application with this new version 
before upgrade.**

* Upgrade cluster version by UI or by CLI
```bash
aws eks --region region --profile aws-zhiwei update-cluster-version --dev-cluster --kubernetes-version 1.11
```

* Upgrade kube-proxy daemonset to use the image that corresponds to the new version of K8s.
```bash
kubectl patch daemonset kube-proxy -n kube-system \
-p '{"spec": {"template": {"spec": {"containers": [{"image": "602401143452.dkr.ecr.us-east-1.amazonaws.com/eks/kube-proxy:v1.11.5","name":"kube-proxy"}]}}}}'
```

* Upgrade DNS 
This is needed when upgrading from 1.10 to 1.11. You'll need to uninstall kube-dns(default DNS in 1.10) and install CoreDNS (default in 1.11).

* Update all worker nodes to the new K8s version.

### Delete
You must delete all services in a cluster and associated resources such as LB before you can celete EKS cluster.

* List all services in a cluster.
```bash
kubctl get svc --all-namespaces
```

* Delete services that have an ssociated with EXTERNAL-IP value. These serivces are associated with ELBs.
```bash
kubectl delete svc service-name
```

* Delete worker nodes. You could just remove cloudformation stack.

* Delete EKS cluster

* Optionally delete VPC cloudformation stack.

## Worker Node
EKS worker nodes are standard EC2 instances. The AMI used is built on top of Amazon Linux 2. 
* It works with EKS out of the box 
* The image includes Docker, kubelet and aws-iam-authenticator. 
* The AMI contains a [bootstrap script](https://github.com/awslabs/amazon-eks-ami/blob/master/files/bootstrap.sh) that discovers ahd connects to cluster's control plane automatically.

The following CloudFormation template is an example on how to create node group.
```text
https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-01-09/amazon-eks-nodegroup.yaml
```

### Launch Worker Nodes
* create node group in cloudformation by [this template](https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-01-09/amazon-eks-nodegroup.yaml)
* to enable worker nodes to join EKS cluster, download [this configuraiton map file](https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-01-09/aws-auth-cm.yaml)
    and run the following command (after filling in instance role ARN, which is created in the previous step).
```bash
kubectl get nodes --watch
```
Wait for all nodes to be in the "ready" state.

If you choose P2 or P3 (GPU) instance type and EKS-optimized AMI with GPU support, you must apply the NVIDIA device
plugin for kubernetes as a daemon set on cluster with the following command:
```bash
kubectl apply -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.11/nvidia-device-plugin.yml
```
### Update
* update worker nodes to use new EKS-optimized AMI
* upgrade K8s version

