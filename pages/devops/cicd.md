---
title: Continuous Integration (CI) and Continuous Deployment (CD)
tags: [devops-exam]
keywords: CI/CD
last_updated: April 14, 2019
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
    * AWS allows you to run these tools on EC2, the spot pricing of AWs can reduce your CI cost.

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

Features for blue and green deployment
* Requires advanced orchestration tooling. 
* Carries significant cost as you have to maintain 2 environments for the duration of deployment.
* Deployment process is rapid, entire environment (blue or green) is deployed all at once. 
* Cutover and migration is clean and controlled (simply a DNS change). 
* Rolling back is equally as clean, DNS regression. 
* Health and performance of the entire green environment can be tested prior to cutover. 
* Using advanced template systems, such as cloud formation, the entire process can be fully automated. 

This method is by far the best from risk mitigation and user impact minimization perspective. 

## AB Testing
With simple blue/green deployment the switchover between blue and green is immediate and binary. Its not used 
to feature test, its used generally to perform full and final updates - unless you have a major issue its one way.
AB testing adjusts this, by sending a percentage of traffic to Blue and a percentage to Green. 

### Process
* Environment starts with 100% of traffic directed at the current (Blue) environment. 
* When green deployment completes, the distribution of traffic is changed to direct a small amount at the new 
    green environment. 
* As confidence builds, the distribution is reversed. 
* Finally 100% of traffic is directed at green, after confidence reaches previous levels the blue environment 
    can be removed. 
    
### Why AB testing?
* It separates different versions of your code, which may have different reliability or performance. 
* It allows a gradual performance/stability/health analysis. 
* It allows a new feature to be tested with a subset of visitors. 
* If bugs are detected, rollback can be enacted quickly and the damage caused by the failed update is minimized. 

The end goal of AB testing might not be migration, it is mainly for testing. 

### How to perform
* Generally, it uses Route 53 - AWS advanced DNS solution.
* Two records are created, one pointing at Blue, the other pointing at Green.
* Traditionally this is called round-robin with a 50/50 request distribution. 
* Weighted-round-robin is an advanced feature where you can specify a weight for each environment. 
* This allows a granular and adjustable distribution such as 100%/0, 30%/70%, 50%/50% or anything in between.
* **IMPORTANT** - it's based on DNS, caching and other DNS related issues can impact the overall accuracy of this 
    technique. 
    
## Bootstrapping
Bootstrapping is a process during which you start with a base image, ISO/AMI etc and via automation build on it to 
    create a more complex object.

Why bootstrapping is powerful?
* With an AMI based approach, you'll need a large number of AMI's. One for each system versions. 

Both AMI-based (everything based into an AMI) or bootstrapping have its own strength. Bootstrapping will take 
    more time to create a working image. 
    
## Immutable Architecture
Immutable Architecture is the practice of replacing infrastructure instead of upgrading or repairing faulty components. 
* Treat service as unchangeable objects. 
* Don't diagnose and fix, throw away and re-create.
* Nothing is bootstrapped, nothing is changed, an image for every version, pre-created, ready to go. 

### Basic Process
* Create a working server using CI/CD process.
* Create an AMI from the working instance.
* From this image, we create our four nodes, and each is an immutable server instance.

### Some principals of immutability
* Traditional architecture treats servers are precious - if they develop a problem, you diagnose, fix and return to service.
* Immutable architecture treats servers as throwaway objects, if a failure is detected, you remove the server, create 
    a new one from an AMI and bring it into service. 
    
## Container/Docker
### Virtualisation vs Containerisation
* A VM contains OS, dependencies and your application. OS will use most of the resource in VM. 
* A container is a lot light weight. It contains dependencies and your application. 

Docker achieves much higher density, and it is much faster to start. 

### Benefits of Docker
* Escape from dependency hell.
* Consistent progression from DEV -> TEST -> QA -> PROD.
* Isolation - performance or stability issues with APP A in container A, won't impact APP B in container B. 
* Resource scheduling is at micro level. 
* Extreme code portability.
* Micro-services.

### Docker Components
* Docker image
* Docker container
* Layers/Union File system
* Dockerfile (each instruction in this file create a layer)
* Docker Daemon/engine
* Docker client
* Docker registries / Docker Hub

