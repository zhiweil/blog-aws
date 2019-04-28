---
title: OpsWorks
tags: [devops-exam]
keywords: AWS OpsWorks
last_updated: April 24, 2019
summary: "OpsWorks is AWS implementation of the CHEF configuration management and automation system."
sidebar: mydoc_sidebar
permalink: devops_opsworks.html
folder: devops
---
OpsWorks allows you to provision infrastructure but abstract some of the details. 

OpsWorks sits between the overhead and configurability of cloudformation (JSON & YAML) but provides more
power and customization than Elastic Beanstalk.

## OpsWorks Structure
### Concepts
* **Stack** - collection of AWS resources that fulfill a purpose, it might a whole application or a subset of it (DEV, CI, QA, PROD etc). 
* **Layer** - a set of shared functionality or components. It is close to the concept of tiers or roles (such web server, DB etc).
* **Instance** - the actual unit of compute (like EC2) within system. It inherits configuration from both stack and layer. The configuration
    of instance is automatic by CHEF. CHEF and OpsWorks are related but not the same thing.
* **Application** - application can be deployed to one or more OpsWorks instances.

### Components
* OpsWorks agent - CHEF - configuration of machines
* OpsWorks automation engine - creating, updating, deleting various AWS infrastructure components. This engine handles
    
    * Load balancing
    * auto-scaling
    * auto-healing   
*Opsworks supports lifecycle events. (this is very important to the exam)
    
**So OpsWorks is the union of CHEF and AWS backend services.**

### Recipes and Cookbooks
OpsWorks and CHEF are declarative desired state engines. 
* You define WHAT should happen and let OpsWorks and CHEF to handle the how.
* Within OpsWorks/CHEF, there are resources such as packages to install and services to start/stop, config file to update.
* Recipes tell OpsWorks what to do.
* Cookbook contains recipes and all associated data to support them.

## Stacks and Layers
To create a stack the following parameters are needed:
* Region
* VPC & default Subnet (this does not prevent you from deploying to other subnets of the VPC)
* OS (Linix or Windows) and version
* Default SSH key
* CHEF version
* Use custom CHEF cookbook (yes/no, default is no).

VPC and region cannot be changed; linux and windows cannot be mixed; default OS verison applies to new instance only.

### Stack resources
The "Resources" tab of stack shows all registered resources with the stack, which include:
* Elastic IPs
* EBS volumes
* RDS - this is identical to adding RDS layer. 

### Layers
Layers are logical grouping of instances, in OpsWorks they share the common configuration elements.

The "Setting" tab of layer include:
* Name & short name
* Instance shutdown time
* Auto-healing enabled (default yes)

The "Recipes" tab of layer include:
* Repository URL
* recipes for lifecycles (each lifecycle can have one or more recipes)

The "Network" table of layer include:
* ELB to a layer
* Public IP or EIP

The "EBS volumes" tab of layer include:
* user EBS optimized volumes
* volumes and mount points

The "Security Group" tab of layer include:
* Security groups
* EC2 instance profile

There are 3 high level layer types:
* OpsWorks - a traditional layer allowing rich feature sets via recipes. 
* ECS - a layer which allows integration of an ECS(docker) cluster with OpsWorks
* RDS - allowing integration between OpsWorks and an existing RDS single instance or HA pair. 

Key points here:
* **A RDS instance can only be associated with one OpsWorks stack.**
* **A stack clone operation does not copy an existing RDS instance.**

## Life cycle events
OpsWorks has five events which occur during the lifetime of an instance. Some events might occur multiple times.
When an event occurs, it runs the recipes assigned to the event. Each layer has its own recipes for each event.

* Setup - Event occurs when an instance has finished booting. This is generally used for instance setup.
* Configure - Enters or Leaves online; associate or disassociate with EIP; attach or detach LB to a layer.
* Deploy - Event occurs when you run the deploy command on an instance.
* Undeploy - Event occurs when you delete an application or run undeploy command.
* Shutdown - Event occurs when an instance is shutdown but before the instance is terminated. It allows cleanup.

## Instance
Instances can be added in two locations:
* layer
* the stack's "Instances" menu

There are 3 high level instance types:
* 24/7 instances - provisioned manually, started and stopped manually via CLI/API.
* Time-based instances - initially privisioned, and configured to power on/off at certain times during the day.
* Load-based instances - initially provisioned, and configured to automatically power on/off based on configurable criteria. 

### 24/7 instances
Some important parameters you can specify on this kind of instances.
* Subnet
* SSH key
* OS
* EBS
* Volume type

### Time-based instances
* The current time is shown as a green reverse triangle. 
* You can set time irrespective of day or a specific day. 
* You time chosen irrespective of day will be shown as green.
* If you select power-on for a specific time of a specific day, it will be shown as light-green.

### Load-based instances
* Prior to setting up load-based instances, you need to setup a per-layer scaling. 
* Simple scaling - based on CPU/MEM/Load
* Complex scaling - based on cloud watch alarms 

## Application
The following parameters define an application:
* Applicaition name
* Document root
* Data source (RDS/None)
* Application Source
    * GIT
    * HTTP
    * S3
* Environment variables
* Domain names
* SSL enabling and setting

### Deploy
Deploying an application in OpsWorks takes the following steps:
* It executes the recipes on the instances targeted by the command (application ID is passed to the command).
* Application parameters are passed to CHEF environment within databag. 
* The deploy recipes access the application source information and pull application payload on to instance.
* 5 versions of an application are maintained; curent and four historic. 

## Create-Deployment
The create-deployment command has two functions:
* create application deployments
* It allows stack level commands to be executed against stack.
**The usage of this command is very important in the exam, it provides far more functionality not limited to deployment.**

### Syntax
```bash
aws opsworks --region ap-southeast-2 create-deployment
```
The options are:
* --stack-id
* --app-id
* --instance-ids
* --comment
* --custom-json
* --generate-cli-skeleton
* --cli-input-json
* --command
    * install_dependencies
    * update_dependencies
    * update_custom_cookbooks
    * execute_recipes
    * upgrade_operating_system
    * configure
    * setup
    * deploy
    * rollback
    * start
    * stop
    * restart
    * undepoly
    
#### Deployment commands
```bash
aws opsworks --region ap-southeast-2 create-deployment 
    --stack-id 11111111-1111-1111-111111111111
    --app-id 11111111-1111-1111-111111111111
    --command "{\"Name\":\"deploy\"}"
```

```bash
aws opsworks --region ap-southeast-2 create-deployment 
    --stack-id 11111111-1111-1111-111111111111
    --app-id 11111111-1111-1111-111111111111
    --command "{\"Name\":\"undeploy\"}"
```

```bash
aws opsworks --region ap-southeast-2 create-deployment 
    --stack-id 11111111-1111-1111-111111111111
    --app-id 11111111-1111-1111-111111111111
    --command "{\"Name\":\"rollback\"}"
```

#### Stack commands
* update_custom_cookbooks - this command does not execute any recipe.
* update_dependencies
* install_dependencies
* execute_recipes
* upgrade_operating_system
* configure
* setup

**Please refer to the user guide of OpsWorks for more details of commands.**

## Databags and BerkShelf
### BerkShelf
For many OpsWorks stacks running old versions (<11.10), you can only specify one cookbook repository. This
significantly limited your ability to use community recipes.  

CHEF 11.10 added BerkShelf, allowing you to install recipes from multiple repositories. 

To enable BerkShelf for OpsWorks:
* Enable the flag on cookbook tab "User custom Chef cookbooks"
* Add a special file to "yourcustomrepo/Berksfile"
    ```text
    source "https://supermarket.chef.io"
    cookbook: 'apt'
    cookbook: 'bleh', git: 'git://somewhere/bleh.git'
    
    cookbook: 'cookbook_name', ['>= cookbook_version'], [cookbook_options]
    ```
    
### Databags
* Databags are global JSON objects accessible from within the CHEF framework. There are multiple databags 
including STACK, LAYER, APP, INSTANCE.
* Databags can be accessed via CHEF methods within compute assets:
    * data_bag_item
    * search
* Databags are constructed with the Custom_JSON field for the above OpsWorks components (STACK, LAYER, APP, INSTANCE).
* Databags (JSON) can contain any JSON objects such as String, Number, Boolean, List and JSON object. 
* The "search" method allows access via an index. OpsWorks offers a range, including the following:
    * aws_opsworks_app - APP databag
    * aws_opsworks_layer - LAYER databag
    * aws_opsworks_instance - INSTANCE databag
    * aws_opsworks_user - USER databag - contains a list of users for a stack
    
```text
app = search(:aws_opsworks_app).first
app_path = "/src/#{app['shortname']}"

package "git" do
    node["platform_version"] == "14.04"
end

application app_path do
    javascript "4"
    environment.update("PORT" => "80")
    environment.update(app["environment"])
    
    git app_path do
        repository app["app_source"]["url"]
        revision app["app_source"]["revision"]
    end
    
    link "#{app_path}/server.js" do
        to "#{app_path}/index.js"
    end
    
    npm_install
    npm_start do
        action [:stop, :enable, :start]
    end
end

```

## Instance auto-healing
* Each OpsWorks instance has an agent installed. 
* Instances perform ongoing heartbeat style health checks with the OpsWorks orchestration engine.
* If heartbeat fails (lost connection for 5 minutes), OpsWorks treats the instance as unhealthy and will perform an auto-heal.
* The actions involved in the auto-heal process will depend on the circumstances under which it is initiated. 

### Auto-healing workflow
* EBS-backed instances - simple stop and start the instance. The instance will go though the following states:
    ```text
    (stop) online -> stopping -> stopped -> (start) requested -> pending -> booting -> online
    ```
* Instance store instance: instance will be terminated and launch a new instance. this will go through the following states:
    ```text
    (terminate) online -> shutting_down -> (launch) requested -> pending -> booting -> online
    ```
    * instance terminated
    * root volume deleted
    * launch new instance - same hostname, config, layer membership
    * reattach any EBS voloumes that were attached when the instance was terminated.
    * Assign a new public and private IP
    * if applicable, re-associates any EIP
    
**In both cases, once back online Opsworks triggers a configure event on all instances.**

### Auto-healing does not do
* Auto-healing won't recover from serious instance corruption. If OpsWorks agent is damaged the instance may fail to 
    to start with a 'start_failed' error. This usually happens for EBS-backed instances.
* This will require manual intervention - either add new instance, or terminate the instance and allow a full re-provision.
* Auto-healing won't upgrade the OS of instance - even if the default OS is changed at a stack level. an auto-healed instance
    will reutrn with the same OS as before the heal.