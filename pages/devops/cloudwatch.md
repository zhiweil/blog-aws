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
