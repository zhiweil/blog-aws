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




### Rolling deployment
You have the control over how many targets to update at each step. At each step x targets are 
updated and health-checked before going to the next step. 
* **Deployment stage #1** - choose x targets to update and health check. 
* **Deployment stage #2** - choose another x targets to update and health check.
* **Deployment stage #3** - choose the last batch of targets to update and health check. 

Features of rolling deployment
* Deployment happens in multiple stages, you decide the nubmer of targets each stage.
* Moving parts, orchestration and health check are required. 
* Overall applicable health isn't necessarily maintained. 
* Can be the least efficent deployment time based on time-taken.
* Allows automated testing, deployment targets are assessed and tested prior to continuing. 
* Generally no down time, assuming number of targets per stage isn't large enough to impact the application.
* Can be paused, allowing limited multi-version testing (combining with small number of targets per stage).

Rolling deployment is considered to the least risk and cheapest deployment method. It is widely used in large scale application deployment. 

### Blue green deployment
Deploy new version of an application to a different environment (separate DNSs), which allows separate assessment and testing. 

* Blue environmet - the environment running old version
* Green environment - the environment running new version

Features:
* Requires advanced orchestration tooling. 
* Carries significant cost as you have to maintain 2 environments for the duration of deployment.
* Deployment process is rapid, entire environment (blue or green) is deployed all at once. 
* Cutover and migration is clean and controlled (simply a DNS change). 
* Rolling back is equally as clean, DNS regression. 
* Health and performance of the entire green environment can be tested prior to cutover. 
* Using advanced template systems, such as cloud formation, the entire process can be fully automated. 

This method is by far the best from rist mitigation and user impact minimization perspective. 