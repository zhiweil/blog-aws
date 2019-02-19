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

## Get Started
When a EKS cluster is created, the user who created the cluster is added to RBAC (role-based access control) table as the
administrator (with system:master permission). Initially, this is the only user can use kubectl to manage the cluster.
* Step #1 - Create EKS cluster - from EKS console.
* Step #2 - Configure kubectl for EKS
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
* Step #3 - Launch and configure EKS worker nodes
**EKS charges you by worker nodes and you must wait for cluster to be ACTIVE before creating worker nodes**
    
    
