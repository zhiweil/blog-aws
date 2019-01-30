---
title:  "VPC Basics"
categories: basic
permalink: networking-cdn-vpc-basics.html
tags: ["Networking"]
---

# Key Concepts
* **Virtual Private Cloud (VPC)** - is the network layer of EC2. VPC is logically isolated from the other CPC on AWS. 
* **Subnet** - A range of IP address in VPC.
* Multiple layers of security
    * **security group**
    * **Access Control List (ACL)**

# Platforms
* **EC2-Classic** - a single falt network that's shared with other customers.
* **EC2-VPC** - accounts created after 2013-12-04 support VPC only.

# Default VPC
* AWS account comes with a **defautl VPC** which has a **default subnet** in each AZ. 
* Default VPC includes a IG, each subnets in default VPC are public subnet. EC2 instances in launched into default VPC gains 
internte access by default.  

# Internet Access
* EC2 instances in public subnet are assigned a public IP and a private IP; those in private subnet are assigned a private IP only. 
* Full IP4 Internet access can be enabled by:
    * Attaching an IG to the VPC
    * Assigned EIPs to EC2 instances 
* To establish IP4 outbound connection from inside VPC, you can use **Network Address Translation (NAT)** device.
* IP6 traffic is separate from IP4 traffic.
    * Full IP6 Internet access address CIDR block can be assigned to VPC, and IP6 addresses can be assigned to EC2
    * To establish IP6 ourbound connection, you can use **egress-only IG**
    
# Accessing Corporate or Home Network
AWS VPC can be connected with your corporate or home network by an **IPsec AWS site-to-site VPN connection**.
* A **virtual private gateway** is attached to VPC.
* A **customer gateway** is connected to corporate or home network.

# PrivateLink
PrivateLink enalbes you privately connect VPC to AWS services of:
* the same account
* VPC endpoint services of other AWS accounts
* supported AWS Marketplace partner services

Traffic between VPC and the services does not leave AWS; it is via Elastic Network Inteface (ENI) and a private IP.

# VPC is PCI DSS compliance 
**PCI DSS** - Payment Card Industry Data Security Standard