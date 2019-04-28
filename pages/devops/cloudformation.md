---
title: CloudFormation
tags: [devops-exam]
keywords: AWS CloudFormation
last_updated: April 18, 2019
summary: "CloudFormation is the main form of infrastructure automation tool."
sidebar: mydoc_sidebar
permalink: devops_cloudformation.html
folder: devops
---
## Basics
### Terms
* **Stack** - cloudformation's unit of grouping for infrastructure.
* **Template** - a JSON document giving cloudformation instructions on how to act and what to create. 
    A template can be used to create and update a stack. 
* **Stack Policy** - IAM style policy statement which governs what can be changed and by who. After a policy 
    is applied to a stack, it cannot be removed but can be updated. 
    
There is a limitation of 200 resources for a single template, but this can be changed. 

### Components
There are nine parts in cloudformation template:

* Version
* Description
* Parameters
* Mappings - hashes (key-value pairs)
* Resources - the only mandatory section.
* Outputs
* Conditions
* Metadata
* Transform

### Capability
* A resource can be created conditionally or the whole template is conditional. 
* CloudFormation can run scripts within instances (EC2). 
* CloudFormaiton can expand files within instances. 
* Each stack has a stack ID, a template can be applied as many times as you want. 
    Each stack and associated resources are unique. 
* The events during stack creation, deletion and update can be configured, meaning 
    cloudformation can be as strict or relaxed; as controlled or as messy as you want. 
    
### When & Where
* Create infrastructure automatically not manually.
* Repeatable environment
* Easily create test environment.
* Define environment once and deploy it to many regions. 
* Manage infrastructure configuration by software development versioning and testing concept.
**A template should be designed to be deployed to one or more regions and as many times as you want.**

## Parameters
Pass one or more values into template. 

Each parameter can have attributes:
* Type: String/Number/List/CommaDelimitedList and lots of AWS specific types that are in user's account such as
    * AWS::EC2::KeyPair::KeyName
    * AWS::EC2::AvailabilityZone::Name
* Default: this value is applied if no value is provided.
* AllowedValues: one or more allowed values.
* AllowedPattern: RegEx which defines the format of a parameter value.
* MinValue & MaxValue
* MinLength & MaxLength
* Description - is displayed on AWS cloudformation stack creation UI.

**It is important to realize that parameters are important, they also remove some 
    of power of cloudformation (name resource, choose VPC and AZs etc).**

**Parameters is a very important topic in exam, so you need to read the corresponding section in User Guide.**

In absence of parameters, cloudformation picks generated names for elements, allowing a single template to be applied 
    repeatedly to an environment.
    
## Resources
* The only mandatory part in cloudformation. It has the following elements:
* Local ID (friendly name) and type must be specified.
* "properties" is a JSON object which contains the attributes of a AWS resource. 
* If name is not specified, cloudformation will deduce a name out of the name of the stack. Not specifying name for 
    resources is one the benefits of cloudformation which allows template reuse.
* In resources, attribute name can reference a parameter (JSON):
    ```json
    {
      "attribute": { "Ref": "parameter"}
    }
    ```

## Output
Outputs are used to record the result of resources allocation of cloudformation result for:
* Display on UI/CLI.
* To be used by parent stack.

Each parameter can be:
* A constructed value
* A parameter
* A pseudo parameter
* An output of a function such as Fn::GetAtt or Ref

## Intrinsic function
Intrinsic functions are inbuilt functions of cloudformation. The intrinsic functions currently available are:
* Fn::Base64 - takes a string and covert it to base64 encoded string such as EC2 user data:
    ```text
    "Fn::Base64": "yum install update && yum install upgrade"
    ```
* Fn::FindInMap - pass in a map object and one or two keys, and return a value.
    ```json
    {
      "mappings": {
        "RegionMap": {"us-east-1": {"32":  "ami-234234", "64":  "ami-255566"}}
      },
      "resources": {
        "MyEC2Instance": {
          "ImageId": {"Fn::FindInMap": ["RegionMap", {"Ref": "AWS::Region"}, "32"]}
        }
      }
    }
    ```
* Fn::GetAtt - get attribute of an object of the current template or the nested templates. The "Ref" function allows
    you to reference to another resource; Fn::GetAtt allows you to reference a property of another resource.
    ```text
    { "Fn::GetAtt": ["Cfn_reosurce_name", "attribute_name"] }
    ```  
* Fn::GetAZs - returns a list of AZs of a region. If region is not specified then this returns a list of AZs fo the region
    where this template is applied to.
    ```text
    { "Fn::GetAZs": "region" }
    { "Fn::GetAZs": "AWS::Region" }
    { "Fn::GetAZs": "" }
    ```
* Fn::Join - this joins a list of strings into a single string with the specified delimeter.
    ```text
    {"Fn::Join": [":", ["a", "b", "c"]] }
    ```
* Fn::Select - select an object from a list of objects. This can be used with other functions like Fn::GetAZs.
    ```text
    { "Fn::Select": ["0", { "Fn::GetAZs": "" }]
    ```
* Ref - reference other properties or resources created in current template. 
    ```text
    "subnet": {"Ref": "MySubnet" }
    ```

And some conditional functions:
* Fn::And
    ```text
    "Fn:And": [{condition1}, {condition2}]
    ```
* Fn::Or
    ```text
    "Fn:Or": [{condition1}, {condition2}]
    ```
* Fn::Not
    ```text
    "Fn:Not": [{condition}]
    ```
* Fn::Equals - usually used to create conditions.
    ```text
    "Conditions": {
      "isProduction": {"Fn:Equals": [{"Ref": "platform"}, "production"]},
      "isDevelopment": {"Fn:Equals": [{"Ref": "platform"}, "development"]}
    }
    ```
* Fn::If - implements the "IF-ELSE" logic. 
   ```text
   "InstanceType": { "Fn:If": ["production", "t2.large", "t2.small"] }
   ```
   
**Please read the Intrinsic functions section of AWS cloudformation user guide.**

## Stack Creation
* S3 upload/S3 template reference
* Template syntax check
* Stack name & Parameter verification & Ingestion
* Cloudformation template processing & Stack creation
    * Resource ordering **by the DependsOn" attributes of resources. The attribute references to another 
        resource in the template but does not need to use the "Ref" function. The "DependsOn" can reference
        to multiple objects.
    * resource generation
    * output generation
    * stack completion or rollback
    
## Resource deletion policy
Resource deletion policy attribue "DeletionPolicy" is associated with all the resource objects in a template. The possible values are:
* Delete (default)
* Retain
* Snapshot (depending on the type of resource)

The resource deletion policy is defined at the top level of an object, not in the "properties" JSON.

### Delete on delete
This creates a transitive environment, which means that the environment can be created or removed without
affecting your wider environment. This might be used in the following scenarios:
* Testing
* CI/CD/QA workflows
* Short life cycle or immutable environment

### Retain on delete
* This is used in traditional infrastructure architectures.
* Windows server platforms (Active Directory)
* Servers with state such as SQL, Exchange, File Server etc. 
* Non immutable architecture
* Negatives - you have less control of billing, resources are still being charged after stack deletion.

### Snapshot on delete
* Only a small amount of resources support this option:
    * AWS::EC2::Volum
    * AWS::RDS::DBCluster
    * AWS::RDS::DBInstance
    * AWS::Redshift::Cluster
* Resources charge stop but you are still being charged for storage.
* You can recover data
* Used when your critical elements are generated data, this allows references or processing at a later time.

## Stack update
* Stack policy is checked. A stack policy consists of the following facts:
    * By default, the absence of a stack policy allows all updates.
    * Once a stack policy is applied it cannot be deleted.
    * Once a stack policy is applied, by default all objects are protected. Update: * is denied.
    * To remove the default DENY protection of an applied stack policy, you need to add a ALLOW policy.
    * A stack policy is a JSON object which is an array of policy objects. Each policy statement has:
        * Effect: Allow/Deny
        * Resource: one or more AWS resources to which the policy is applied. Or inverted resources "NotResource".
        * Principal: this is a "*".
        * Action: the actions allowed/denied such as "Update: *".
        * Condition: "Resource" allows single or wild card matching against LogicalId; "Condition" allows the 
            same based on resource type.
        ```json
        {
          "Statement": {
            "Condition": {
              "StringEquals": {
                "ResourceType": [
                  "AWS::EC2::Instance",
                  "AWS::RDS::DBInstance"
                ]
              }
            }
          }
        }
        ```  
* Resource update is different from resource type to type. The update can be:
    * No interruption (no instance reboot for example).
    * Some interruption
    * Replacement
    * Delete
    ```text
    AWS::EC2::Instance
        AvailabilityZone -> replacement as insances cannot be moved to another AZ.
        EbsOptimiced -> some interruption (EBS backed), as instance need to shut down and restart again.
        ImageId -> replacement
        InstanceType -> some interruption (EBS backed), as this needs to shut down, change and restart.
    AWS::DynamoDB::Table
        TableName -> replacement
        ProvisionedThroughput -> no interruption
    AWS::Autoscaling::LaunchConfiguration
        Everything requires replacement.
    ```    
    * Please refer to "AWS Resource type reference" section of cloudformation user guide for more details.
    
## Stack Nesting
With stack nesting, a resource can be a whole stack nested within a parent template. Nested stack can itself
have nested stack. 
* It allows potentially huge set of infrastructure to be split over multiple templates.
* Remember there is a size limit to a single stack/template.
    * 460kB template limit, assuming in S3 (from CLI this is even smaller).
    * 200 resources per template/stack.
    * 100 mappings, 60 parameters, 60 output limits per template/stack.
* By nesting, it allows more effective infrastructure-as-code reuse.
* One template is unwieldy, you may not need every component. 
    * You can split off infrastructure and AD.
    * You can split off infrastructure and SQL.
    * You can split off sharepoint
    
The following is an example of reusing SQL server template.
```json
{
  "SQLStack": {
    "Type": "AWS::CloudFormation::Stack",
    "Properties": {
      "TemplateURL": "https://s3.etc.etc/template.json",
      "Parameters": {
        "VPC": { "Fn::GetAttr":  ["AD", "Outputs.VPC"]}
      }
    } 
  }
}
```

## Creation policies, Wait condition & Handler
For simple or AWS managed resources, the "DependsOn" works fine. But for complicated dependency chain, 
This won't work. 

The following three mechanisms accomplish the same task - when a resource should be marked as "completed". 
* Creation policy - can only be used with EC2 instances and auto-scaling groups.
* Wait condition
* Wait condition handler

Wait condition and wait condition policy can be used in much complex scenarios. 

### Creation Policy
There are two components for creation policy.
* Creation policy definition within EC2 instances or auto-scaling groups. Two important properties are the 
    "DesiredCapacity" and "Count" properties on the AWS resources to be created.
* The signal configuration within the EC2 intance user data or lauch configuration user data.
* When the number of signals is equal to or more than the "Count" in creation policy, the resource is marked 
    as complete.
    
```json
{
  "AutoscalingGroup": {
    "Type": "AWS::Autoscaling::AutoscalingGroup",
    "Properties": {
      "DesiredCapacity": "2",
      "LaunchConfigurationName": "MyLC"
    },
    "CreationPolicy": {
      "ResourceSignal": {
        "Count": "2",
        "Timeout": "PT15M"
      }
    }
  },
  "MyLC": {
    "Type": "AWS::Autoscaling::LaunchConfiguration",
    "UserData": {
      "Fn::Base64": {
        "Fn:Join": [
          "",
          [
            "cnf-signal -e --stack ",
            {"Ref": "AWS::StackName"},
            " --resource AutoscalingGroup ",
            "--region ",
            {"Ref": "AWS::Region"}, 
            "\n"
          ]
        ]
      }
    }
  }
}
```

### Wait Condition & Handler
* A wait condition handler is a cloudformation resource with no properties, but it generates a pre-signed URL which
    can be used to communicate SUCCESS or FAILURE.
* Wait condition has four components:
    * "DependsOn" other resource(s)
    * A hanle properties, which refers to a wait condition handler.
    * A response timeout
    * A "Count", default is 1 if not specified.
```json
{
    "waitHandle": {
      "Type": "AWS::CloudFormation::WaitConditionHandle"
    },
    "waitCondition": {
      "Type": "AWS::CloudFormation::WaitCondition",
      "DependsOn": {"Ref":  "MyEC2"},
      "Properties": {
        "Handle": {"Ref": "waitHandle"},
        "TimeOut": "3600",
        "Count": "1"
      }
    },
    "MyEC2": {
      "Type": "AWS::EC2::Instance",
      "Properties": {
        "Userdata": {"Fn::Base64": {"Fn:Join": [
          "", ["cmd", " ", "URL=", {"Ref": "waitHandle"}]
        ]}}
      }
    }
}
```

How wait condition & Handler is different from creation policy?
* Can have complex order. It can reply on many resources and have many reosurces relied on it.
* The "DependsOn" property of "WaitCondition" allows you to control the order.
* Additional data can be passed back via signed URL.
    ```json
    {
      "Status": "statusValue",
      "UniqueId": "unique ID",
      "Data": "data",
      "Reason": "reason"
    }
    ```
* Any of the value above can be assessed within template by:
    ```json
    {"Fn::GetAtt":  ["waitCondition", "Data"]}
    ```
    
## Custom Resources
Why custom resources?
* Not all features of all AWS services are supported by cloudformation.
* Cannot operate on non-AWS resources.
* Although you have some logic/operator/funtion, its ability to act programatically outside resources is limited.
* Its ability to interact with external services as part of stack operations is limited. 

**None of the points above are not actually true.** - by using custom resources.

### Definition
It's a resoure type within cloudformation template that is backed by SNS and Lambda. 

We will focus on SNS as Lambda backed custom resources are only supported in some regions (those have Lambda support). 

```json
{
    "Type": "Custom::resourceNameHere",
    "ServiceToken": "arn:aws:sns:ap-southease-2:12345678910:snp"
}
```

When a stack is created, updated or deleted, a SNS notification message will be sent to a SNS topic containing
the event and a payload (or a Lambda function is invoked with the same information). 

The basic work flow is:
```text
CloudFormation -> SNS topic -> Lambda function or EC2 worker instance or external appliation/configuration management system 
```

### Example - VPC for QA/test environment
A cloudforamtion template which aquires subnets range automatically. Store the generated subnets in DynamoDB table and remove then when stack is deleted.

* Create a custom resource, when it is created, updated or deleted, a message will be posted to a SNS topic.
```json
{
  "CustomLambdaResourceSubnetPicker": {
    "Type": "Custome::LambdaSubnetLookup",
    "Properties": {
      "ServiceToken": "arn:aws:sns:ap-southease-2:12345678910:snp",
      "Cidr": "10.16.128.0/17"
    }
  }
}
```
* Create a lambda function which runs when there is a new notification to a SNS topic. The message is:
```json
{
  "RequestType": "Create", 
  "ResponseURL": "https://pre-signed-s3-url-for-response",
  "StackId": "arn:aws:cloudformation:ap-southeast-2:12345678910:stack/stackname/guid",
  "RequestId": "uuid of this request",
  "ResourceType": "Custome::LambdaSubnetLookup",
  "LogicalResourceId": "CustomeLambdaResourceSubnetPicker",
  "ResourceProperties": {
    "Cidr": "10.16.128.0/17"
  }
}
```
* Lambda function will process the request above as needed and then respond by by "ResponseURL" in the original request.
```json
{
  "Status": "SUCCESS",
  "StackId": "stack ID",
  "RequestId": "request ID",
  "LogicalResourceId": "logical resource ID",
  "PhysicalResourceId": "physical resourde ID",
  "Data": {
    "PublicSubnet": "10.16.128.0/25",
    "PrivateSubnet": "10.16.128.128/25"
  }
}
```
* Reference custome resource in cloudformation template
```json
{
  "PublicSubnet": {
    "Type": "AWS::EC2::Subnet",
    "Properties": {
      "CidrBlock": {
        "Fn::GetAtt": [
          "CustomeLambdaResourceSubnetPicker", "PublicSubnet"
        ]
      },
      "MapPublicIpOnLaunch": true,
      "VpcId": "vpc id"
    },
    "DependsOn": "CustomeLambdaResourceSubnetPicker"
  }
}
```

### Use cases
* Stack linked to on-premise resource creation
* Stack linked to advanced logic - resource discovery.
* Stack deletion linked to advanced tidy operations - backups/monitoring/deactivation etc.
* Stack linked to on-premise configuration management system.
* Web stack creation - linked to external monitoring/penetration testing system.
* Stack creationg/deletion updates a Lambda based backup solution - EBS snapshotting.
* Stack deletion spawns account wide pruning for orphaned EBS volumes. 
