---
title: Auto-Scaling
tags: [devops-exam]
keywords: AWS OpsWorks
last_updated: May 15, 2019
summary: "AWS auto-scaling related services"
sidebar: mydoc_sidebar
permalink: auto_scaling.html
folder: devops
---

## Basics
* Scale EC2 instance capacity (UP or Down) automatically according to your conditions. 
* Increase instances during demand spikes, maintain performance, decrease capacity during lulls. 
* Save money.
* Suitable for stable demand applications or hourly/daily/weekly demand fluctuations. 

### Key components:
* Auto-scaling group - EC2 instances are organized in groups. You can specify:
    * maximum number of instances
    * minimum instances
    * desired number of instances
* Auto-scaling configuration - Each group uses a configuration for launching EC2 instances.
* Scaling plan - How and when to scale. This can be based on conditions (dynamic) or time (scheduled). 

### Benefits
 * Better fault tolerance 
 * Better Availability
 * Better cost management
 
### Limits
Resources | Limit |
----------|-------|
Launch Configurations | 100 |
Autoscaling groups | 20 |
Lifecycle hooks per group | 50 |
LBs per group | 100 (attached max 10 LBs at a time)|
Step adjustments per group | 20 |

**NOTE: these limits can be increased**

