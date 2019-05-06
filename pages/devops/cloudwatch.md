---
title: CloudWatch
tags: [devops-exam]
keywords: CloudWatch
last_updated: April 30, 2019
summary: "CloudWatch is a metric gathering, monitoring & alerting, graph service for AWS"
sidebar: mydoc_sidebar
permalink: devops_cloudwatch.html
folder: devops
---

## Basics
* We have pre-defined metrics AWS creates for us for many of their services.
* We have dimensions, which are alternate ways to look at the metric data. 
* **CloudWatch data for metrics will be kept for as long as two weeks.** If you do want to keep you metric 
data for more than two weeks you'll need to do something special.
* **Name Spaces** are containers of metrics. Metrics are put under namespaces so they aren't accidentally aggregated.
* To be able to view the "EC2 Metrics", you'll need to enable "Detailed monitoring" on EC2 instances.
* You can aggregate metrics by auto-scaling group.

**Read the entire developer guide!**

## Custom Metrics
The following example create a metric for current utc epoch time under namespace "myns".
```bash
aws cloudwatch put-metric-data --metric-name CurrentUnixTime --namespace "myns"  
    --value `date +%s` 
    --timestamp `date --utc "+%FT%T.%N" | sed -r 's/[[:digit:]]{6}$/Z'`
``` 

## Alarms
* Initiate actions on your behalf
* Based on parameters you specify
* Against metrics you have in use

Alarms are send to the SNS for notification; to Auto-scaling group for action such as scale up/down.

### Notes
* Alarm period should be greater than or equal to metric frequency. 
    * Basic monitoring give you frequency of 5 minutes.
    * Detailed monitoring give you frequency of 1 minute.
* Alarms can't invoke an action because they are in a state, the state must change.
* Alarms actions must be in the same region as the alarm.
* Some AWS resources don't send metric date to CloudWatch under certain conditions (insufficient data state).
    For example, when EBS volumes are not attached to EC2.

### State
* OK - metric data is within the threshold defined.
* ALARM - metric data is outside the threshold defined.
* INSUFFICIENT_DATA - either metric does not have data or the data is no enough to determine alarm state.

### Restrictions and Commands
* upto 5000 alarms per account
* create and update alarm by mon-put-metric-alarm
* enable or disable alarm by mon-[enable|disable]-alarm
* describe alarms by mon-describe-alarms
* you can create alarm before you have created metric, but you'll need metric's attributes to create the alarm 

### Example
* The following command is used to create an alarm for metric "CurrentUnixTime". 
```bash
aws cloudwatch put-metric-alarm    
    --alarm-name TimeAlarm
    --alarm-description "Alert at a certian time"
    --metric CurrentUnixTime
    --namespace myns
    --statistic "Average"
    --period 60
    --threshold 123456
    --comparasion-operator GreaterThanOrEqualToThreshold
    --evaluation-period 1
    --alarm-actions "arn:aws:sns:sp-southeast-2:11111111:alarm-responders"
```

### Autoscaling based on alarms
```bash
# create a launch configuration
aws autoscaling create-launch-configuration
    --launch-configuration-name my-lc
    --image-id ami-xxxxxxx
    --instance-type m2.medium
    
# create autoscaling group
aws autoscaling create-auto-scaling-group
    --auto-scaling-group-name my-asg
    --launch-configuration-name my-lc
    -- max-size 5 --min-size 1
    --availability-zones ap-southeast-2a
    
# create autoscaling policies
aws autoscaling put-scaling-policy 
    --policy-name my-scaleout-policy
    --auto-scaling-group-name my-asg
    --scaling-adjustment 30
    --adjustment-type PercentChangeInCapacity
aws autoscaling put-scaling-policy 
    --policy-name my-scalein-policy
    --auto-scaling-group-name my-asg
    --scaling-adjustment -2
    --adjustment-type ChangeInCapacity
    
# Create alarms by CPU usage
aws cloudwatch put-metric-alarm     
    --alarm-name AddCapacity
    --metric-name CPUUtilization 
    --namespace AWS/EC2
    --statistic Average --period 60
    --threshold 80
    --comparison-operator GreatherThanOrEqualToThreahold
    --dimensions "Name=AutoscalingGroupName, Value=my-asg"
    --evaluation-period 2
    --alarm-actions "arn:aws:autoscaling:111111:scaleout-policy-created-in-the-previous-step"
aws cloudwatch put-metric-alarm     
    --alarm-name RemoveCapacity
    --metric-name CPUUtilization 
    --namespace AWS/EC2
    --statistic Average --period 60
    --threshold 40
    --comparison-operator LessThanOrEqualToThreahold
    --dimensions "Name=AutoscalingGroupName, Value=my-asg"
    --evaluation-period 2
    --alarm-actions "arn:aws:autoscaling:111111:scalein-policy-created-in-the-previous-step"
```

A useful tool called "stress" can be used to simulate high CPU/MEM/Network usage. It can be installed by:
```bash
sudo dnf install stress
```
## Logs
CloudWatch logs allow you to monitor your system, application and custom logs in **real time**.
* Send your existing logs to CloudWatch
* Create patterns to look for in your logs
* Create alerts based on the findings of patterns you defined

CloudWatch provides free agents for 
* Ubuntu
* Amazon Linux
* Windows

### Three Main Purposes
1. Monitor logs from EC2 instances in real time.
2. Monitor CloudTrail logged events
3. Archive log data

### Terminologies
* Log event - a record sent to CloudWatch logs to be stored. Timestamp and message.
* Log Stream - this is a sequence of log events that share the same source. Log streams are
    automatically deleted after two months. 
* Log group - a group of log streams that share the same retention, monitoring and access 
    control settings. All log streams have to belong to a log group.
* Metric filter - defines how a service would extract metric observations from events and turn
    them into data points for a CloudWatch metric. Metric filters can be assigned to log streams and
    log groups.
* Retention setting - defines how long log events are kept in CloudWatch logs. Expired logs are deleted 
    automatically. Retention settings are assigned to log groups, which apply to their streams.

### Install CloudWatch agent
```bash
wget https://s3.amazonaws.com/aws-cloudwatch/downloads/latest/awslogs-agent-setup.py
sudo python ./awslogs-agent-setup.py --region ap-southeast-2

# after answering a couple of questions, the awslogs service is installed and up running
# NOTE: it is suggested to assign a role to your EC2 instances rather than using access credential
# you can check the status of awslogs service by
sudo service awslogs status
```

### Log filters
* Defines the patterns to look for in logs
* Can be used to turn into metrics
* Can then graph and set alarm on those metrics

NOTE:
* Filters won't work on existing data; they only work on data passed in after filters are defined.
* Filters only return the first 50 results.

A metric filter consists fo the following parts:
* Filter Pattern
* Metric name
* Metric namespace
* Metric Value (count or number) 

**The workflow is: log -> CloudWatch Logs -> CloudWatch Log filter -> CloudWatch Alarm**

## Real time log processing
AWS services can subscribe to CloudWatch logs and capture/process/analyse them in real time. 
These services incoude:
* Kinesis streams (formerly known as kinesis)
* Lambda
* Kinesis Firehose

**Please refer to the "Subscriptions" section of CloudWatch Developer Guide for more details.**

### Example
* create a kinesis stream & view its details & record stream ARN
```bash
aws kinesis create-stream --stream-name "AlarmLogs" --shard-count 1
aws kinesis describe-stream --stream-name "AlarmLogs"
```

* The following policy allows AWS CloudWatch logs to assume role for kinesis 
```json
{
  "Statement": {
    "Effect": "Allow",
    "Principal": {"Service":  "logs.ap-southeast-2.amazonaws.com"},
    "Action": "sts:AssumeRole"
  }
}
```
```bash
aws iam create-role --role-name "CWLtoKinesisRole"     
    --assume-role-policy-document file://path/to/policy.json
```

* Create permissions for EC2
```json
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "Kinessis:PutRecord",
      "Resource": "stream ARN"
    },
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource":"CWLtoKinesisRole ARN" 
    }
  ]
}
```

```bash
aws iam put-role-policy --role-name "CWLtoKinesisRole"    
    --policy-name "Permission-Policy-for-CWL"
    --policy-document file://path/to/policy.json
```

* Create Subscription filter
```bash
aws logs put-subscription-filter --log-group-name "my-lg"
    --filter-name "my-filter"
    --filter-pattern "Invalid User"
    --destination-arn "arn:aws:kinesis:ap-southeast-2:1111111:stream/AlarmsLogs"
    --role-arn "arn:aws:iam:ap-southeast-2:111111:role/CWLtoKinesisRole"
```