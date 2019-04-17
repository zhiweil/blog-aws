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
* This is not being widely used these days. 
* The deployment involves a single server (non-HA).
* For small development teams.

When there is a new version of application, the deployment happens. The deployment has the following drawbacks:
* Brief outage occurs when new version is being installed.
* There is no secondary server, so testing opportunity is limited. 
* Rollback involves removing the old version and installing the previous one. 

### All-at-once deployment
* Deployment happens in one step (same as single target deployment).
* Multiple targets.
* More complicated than single target deployment, so orchestration is needed. 

All-at-one deployment shares the same drawbacks with single target deployment: no ability to test, 
short period of outage and less than ideal rollback.

**This method may be used with non-critial applications, with 5 to 10 targets.**

### Minimum in-service deployment
The minimal (optimal) number of targets can be specified with orchestration server. Orchestration 
sever makes sure these targets are always available in deployment. 

The deployment follows the following process:
* **Initial Build Stage** - all targets are running the old version, the orchestration server is told the minimum number 
    of running targets it should keep (in AWS, this might be one instance in each AZ). 
* **Deployment Stage #1** - the minimum number of targets keep running the old version; all the other targets are upgrading.
* **Deployment Stage #2** - After upgrading some targets, orchestration server still keeps running the minimum number
    of targets and upgrading the ones running the old versions.  
* **Deployment Complete** - All targets have been updated to the new version.

The features:
* Deployment happens in multiple stages.
* Deployment happens to as many targets as possible while maintaining the minimum in-service targets. 
* A few moving parts, orchestration and health checks are required. 
* Allows automated testing, deployment targets are assessed and tested prior to continuing. 
* Generally no downtime. 
* Often quicker and less stages than rolling deployment. 

### Rolling deployment
This deployment allows you to control the stages of the deployment. 

