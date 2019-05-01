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

