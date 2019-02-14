---
title: VPC
tags: [Networking]
keywords: AWS VPC
last_updated: February 14, 2019
summary: "The key components of AWS VPC security include security group, network ACL and flow logs."
sidebar: mydoc_sidebar
permalink: vpc_security.html
folder: Networking
---

## Comparison of security group and network ACL
Security Group | Network ACL|
---------------|------------|
applis to EC2 instance | applies to subnet |
supports allow rules only | supports both allow and deny rules |
stateful, return traffic is automatically allowed | stateless, have to configure inbound and outbound separately |
all rules are evaluated before decision | process rules in number order |
have to configure EC2 instance explicitly | apply automatically to all EC2 of a subnet |

## Security group
**Can assign up to 5 security groups to an EC2.** If no security group is specified when launching an EC2, the default security group of VPC applies.

### Basics
* allow rules only
* can specify inbound and outbound rules separately
* by default, a SG has no inbound rule, so no one can access it
* by default, a SG has a outbound rule for everything
* SG rules are stateful
* instances associated with the same SG cannot talk to each other until it is allowed by adding a rule
* SG is associated with ENI. 

### Default security group
* you cannot delete default SG of VPC
* default SG allows inbound traffic from all EC2 instances of the same SG.
* default SG allows all outbound traffic (IPv4 & v6)

### Some Useful Port 
* 80 - http
* 443 - https
* 22 - ssh
* 3389 - RDP
* 1433 - MS SQL Server
* 3306 My SQL
* 5432 PostgreSQL

## Network ACL
### Basics
* VPC has a default ACL (modifiable), which allows all inbound and outbound traffic (IP v4 & v6)
* by default, a new ACL denies all inbound and outbound traffic until you add rules.
* a new subnet must be associated with a ACL, default ACL will be applied if not specified
* a ACL can be associated with multiple subnets, but a subnet can be associated with only one ACL
* the rules of ACL is evaluated in order
* ACL can have separate inbound and outbound rules, each rule can allow or deny traffic
* ACL rules are stateless

### Default ACL
Default ACL of VPC allows all inbound and outbound traffic (IP v4 & v6).
There is a default rule, with the "*" as the rule number, cannot be removed.

