---
title: Elastic Beanstalk
tags: [devops-exam]
keywords: AWS Elastic Beanstalk
last_updated: April 27, 2019
summary: "Elastic Beanstalk provides developers or end users with the ability to provision application infrastructure in an almost transparent way."
sidebar: mydoc_sidebar
permalink: devops_elasticbeanstalk.html
folder: devops
---

Elastic Beanstalk focus on components and performance, not configuration and specification. It
attempts to remove, or significantly simplify infrastructure management, allowing applications
to be deployed into infrastructure environments easily.

## Key architecture components
* **Application** - the high level structure in beanstalk. 
    * Either you entire application is one EB application 
    * or each logical component of your application, can be a EB application or a EB environment within an application
    * Each application has its own URL, the URL can be switched among different environments (blue-green deployment). 
* Application can have multiple **environments**. 
    * Environment can be development stages such as Prod, Staging, Dev, V1, V2 
        or functional type such as front-end, back-end.
    * Environments are either single instance or scalable.
    * Environments are either web server environments or worker environments.
* **Application versions** are unique packages which represent versions of applications. 
    * One application can have multiple application versions. 
    * Application versions can be deployed to environments within an application.
    * An application (version) is uploaded to EB as an **application bundle(ZIP file)**.

## Environment types
* **Web server environment** - Web servers running inside EC2 instances which are running inside a auto-scaling group and behind a LB. Web servers might 
be communicating with worker environment via SQS queues (messaging), the two environments are loosely coupled. This environment usually pushes messages to 
SQS queues.
* **Worker environment** - Rather than having LB, this environment is auto-scaled via cloud watch alarms. So this environment is a multi-server environment. 
Worker services run inside EC2 instances, which run inside auto-scaling groups. This environment usually scales by the SQS queues, them more messages in the 
queues, the more instances will be created. The queues are handled by a EB daemon, so you don't need to care about setting the parameters of queues (such as 
invisibility timeout). 

## Supported Environments
* Node.js
* PHP
* Python
* Ruby
* Apache Tomcat
* Microsoft IIS
* Java
* Go
* Docker

## Extending EB by .ebextensions
".ebextensions" is a folder in EB source bundle. It allows granular configuration of EB environment and customization of 
the resources it contains (ELB, EC2 etc). 

Files within .ebextensions are YAML format and end with a ".config" extension. The ".config" files contain a number of sections.
* option_settings - allow you to declare global configuration options. 
* resources - allow you to specify additional resources to provision in your environment, or define granular configuration on those 
    resources.
* Other sections, packages, sources, files, users, groups, commands, container_commands and service allow customization
    of the EC2 instances as part of your environment - similar to the cloudformation AWS::CloudFormation::Init
    directive.

### Leader instance
A leader instance is an EC2 instance within a load-balancing, autoscaling environment, chosen to be the leader/master.

EB has this leader/master instance concept during environment creation. Once the environment is established, all nodes are equal.

The "leader_only" directive can be used on with the container_commands section of a .config file within .ebextensions.
    This ensures a command runs only once and on the leader node only.
```yaml
container_commands:
    name_of_container_command:
      command: "command to run"
      leader_only: true
```

### Example of .config
A .config file has two sections: sources and commands.
```yaml
sources: 
    /aws-scripts-mon: http://ec2-downloads.s3.amazonaws.com/cloudwatch-samples/CloudWatchMonitoringScripts.zip
    
commands:
    01-setupcron:
      command: echo "*/5 * * * * * root perl /aws-scripts-mon/mon-put-instance-data.pl --mem-used > /dev/null" > /etc/cron.d/cwpump

    02-changeperm:
      command: chmod 644 /etc/cron.d/cwpump
```

### Examples of .ebextensions
```yaml
users:
    - myuser:
          groups:
            - group1
            - group2
          uid: 50
          homedir: "/tmp"
groups:
    - group1: 45
    - group2: 99
```

```yaml
services: 
 sysvinit:
  - myservice:
      enabled: true
      ensureRunning: true
      
Resources: 
    DDBTable:
      Type: AWS::DynamoDB::Table
      Properties:
        KeySchema:
          HashKeyElement:
            AttributeName: id
            AttributeType: S
        ProvisionedThroughput:
          ReadCapacityUnits: 1
          WriteCapacityUnits: 1
    MySQSQueue:
      Type: AWS::SQS::Queue
      Properties:
        VisibilityTimeout:
          Fn::GetOptionSetting:
            OptionName: VisibilityTimeout
            DefaultValue: 30
```
You can use Ref and Fn::GetAtt to reference resources and inject those values into configuration files. 

## Docker in EB
Docker can be used to support the environments that are not yet supported by AWS. This includes following components:
* Application source bundle
    * application source
    * dependencies
    * scripts
    * .ebextensions
* Dockerfile - contains the structure of docker container. Only need this for build a custom container on each EC2 instance.
    * source image
    * how the elements of source bundle are injected into the image
    * how to expose ports on which the container listens via the EXPOSE directive
* Dockerrun.aws.json - EB specific file. 
    * defines how to deploy a docker as a EB application
    * defines the ports the container is listening on, allowing EB to map these to standard ports
    * defines authentication information to a .dockercfg file for private docker registry.
    * Allows EC2 <-> container mapping
    * points EB at where your application logs.
* dockercfg - contains authentication information for private docker registry.
    * stored on S3
    * Use "docker login registry-url" to generate config.json or .dockercfg (version dependant). Use this file
        as a basis, create an auth file in a S3 bucket. This bucket must be in the same region as EB environment.
        ```text
        # in dockerrun.aws.json
          "Authentication": {
            "Bucket": "my-bucket",
            "Key": "mydockercfg"
          }

        # mydockercfg
        "server": {
          "auth": "auth_token",
          "email": "email@server.com"
        }
        ```    