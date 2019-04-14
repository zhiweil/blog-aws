---
title: Continuous Integration (CI) and Continuous Deployment (CD)
tags: [Networking]
keywords: AWS VPC
last_updated: July 16, 2016
summary: "Basic concepts of CI and CD"
sidebar: mydoc_sidebar
permalink: devops_cicd.html
folder: devops
---
## Basics
Traditional development process allows developers to develop separately, which causes issues when
integrating code together. This is referred to "Integration Hell". The longer the code remains checked-out
the higher the chance for this to cause problems in integration. 

**CI**- This process is designed to highlight integration issues earlier. This process started from code commit, and
    followed by automated build and test (unit & component & integration testing). The tools that support CI:
    
    * Bamboo
    * Cruise Control
    * Gitlab
    * Jenkins
    * Go
    * Teamcity
    * AWS allows you to run these tools on EC3, the spot pricing of AWs can reduce your CI cost.

**CD** - This is a work-flow based process which takes the output of CI (a tested software) and automatically
    deploy it to a QA, pre-production (staging) and production environments. Most of the CI servers 
    listed above supports some form of CD. 
    
AWS provides the following services for CD:

* CodeDeploy
* CodePipeline

In AWS, CD tools can utilise the following services in integration and deployment:

* CloudFormation
* Elastic Beanstalk
* ECS
* EKS

## Deployment Types
### Single Target Deployment
This is not being widely used these days. 