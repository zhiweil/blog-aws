---
title: EKS Basics
tags: [computing]
keywords: AWS Computing EKS Kubernetes
last_updated: Feb 19, 2019
summary: "Amazon Elastic Container Service for Kubernetes (EKS) is a managed service for automating, scaling and management of containerized applications"
sidebar: mydoc_sidebar
permalink: eks_basics.html
folder: computing
---
To work with EKS, you would take the following steps:
1. create EKS cluster and EKS deploys K8s masters for you automatically
2. Launch worker nodes that register with EKS cluster
3. User K8s tools such as kubectl to communicate with the created cluster
4. deploy and manage application on cluster the same way as the other K8s environment

Pre-requsites before using EKS
* A service role must exist before using EKS which **allows EKS to manage your clusters on your behalf**.
* It is recommended to put EKS cluster in its own VPC, so create VPC for a EKS cluster. 
    [This Cloudformation template](https://amazon-eks.s3-us-west-2.amazonaws.com/cloudformation/2019-01-09/amazon-eks-vpc-sample.yaml) 
    creates a VPC for you.
* [install and configure kubectl or EKS](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl). 
* [install aws-iam-authenticator for Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html)

When a EKS cluster is created, the user who created the cluster is added to RBAC (role-based access control) table as the
administrator (with system:master permission). Initially, this is the only user can use kubectl to manage the cluster.

## Step #1 - Create EKS cluster
This can be created from either console or CLI like below

```bash
aws --region us-east-1 --profile aws-zhiwei eks create-cluster --name dev-cluster \
 --role-arn arn:aws:iam::457964311029:role/eksServiceRole \
 --resources-vpc-config subnetIds=subnet-04b02dd061f4129e4,subnet-0ac348a0978bd47ca,subnet-0be7c84985cc78393,securityGroupdIds=sg-011fb571c9fc31fcc
```

## Step #2 - Configure kubectl for EKS

* create or update EKS k8s config by
```bash
aws eks --region us-east-1 --profile aws-zhiwei update-kubeconfig --name dev-cluster
```
* config file is created at $HOME/.kube/config
* test configuration by
```bash
kubectl get svc
```

* Trouble Shooting
    * Get a EKS token manually by
    ```bash
    aws-iam-authenticator token -i cluster-name
    ```
    * Verify EKS token by
    ```bash
    aws-iam-authenticator verify -t k8s-aws-v1.really_long_token -i cluster-name
    ```
    * Since kubectl always try to use environment variable for authentication, should set the following credential by 
        the user that creates EKS cluster if you are not using the default aws-cli profile.
        * AWS_ACCESS_KEY_ID
        * AWS_SECRET_ACCESS_KEY

## Step #3 - Launch and configure EKS worker nodes
**EKS charges you by worker nodes and you must wait for cluster to be ACTIVE before creating worker nodes**
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
    
## Step #4 - Launch an application
Refer to [this Github page](https://github.com/kubernetes/examples/blob/master/guestbook-go/README.md) for more details about setting up the guest book example.    

To launch the application, you will need to run the following command in order
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/redis-master-controller.json
kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/redis-master-service.json
kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/redis-slave-controller.json
kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/redis-slave-service.json
kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/guestbook-controller.json
kubectl apply -f https://raw.githubusercontent.com/kubernetes/examples/master/guestbook-go/guestbook-service.json
```    

Run the following command and wait until the "External IP" column is populated for the "guestbook" service.
```bash
kubectl get services -o wide
```
You'll need to wait several minutes for DNS name is populated. 

**In the end, open the "External-IP" in a browser!** 
If you you are unable to connect to "http://External-IP:3000" from browser, make sure that your firewall is not blocking 
non-standard port such as 3000, on which the "guestbook" service is running. 

## Step #5 - Cleanup
* delete "guestbook" service objects
```bash
kubectl delete rc/redis-master rc/redis-slave rc/guestbook svc/redis-master svc/redis-slave svc/guestbook
```
* delete the cloudformaiton stacks created previously

