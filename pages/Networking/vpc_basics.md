---
title: VPC Basics
tags: [Networking]
keywords: AWS VPC
last_updated: July 16, 2016
summary: "Amazon Web Service Virtual Private Cloud (VPC) is the networking service."
sidebar: mydoc_sidebar
permalink: vpc_basics.html
folder: Networking
---

## Key Concepts
* **Virtual Private Cloud (VPC)** - is the network layer of EC2. VPC is logically isolated from the other VPC on AWS. 
* **Subnet** - A range of IP address in VPC.
* Multiple layers of security
    * **security group**
    * **Access Control List (ACL)**

### Platforms
* **EC2-Classic** - a single falt network that's shared with other AWS customers.
* **EC2-VPC** - accounts created after 2013-12-04 support VPC only.

### Default VPC
* AWS account comes with a **defautl VPC** which has a **default subnet** in each AZ. 
* Default VPC includes a IG, each subnets in default VPC are public subnet. EC2 instances in launched into default VPC gain 
internte access by default.  

### Internet Access
* EC2 instances in public subnet are assigned a public IP and a private IP; those in private subnet are assigned a private IP only. 
* Full IP4 Internet access can be enabled by:
    * Attaching an IG to the VPC
    * Assigned EIPs to EC2 instances 
* To establish IP4 outbound connection from inside VPC, you can use **Network Address Translation (NAT)** device.
* IP6 traffic is separate from IP4 traffic.
    * Full IP6 Internet access address CIDR block can be assigned to VPC, and IP6 addresses can be assigned to EC2
    * To establish IP6 outbound connection, you can use **egress-only IG**
    
### Accessing Corporate or Home Network
AWS VPC can be connected with your corporate or home network by an **IPsec AWS site-to-site VPN connection**.
* A **virtual private gateway** is attached to VPC.
* A **customer gateway** is connected to corporate or home network.

### PrivateLink
PrivateLink enalbes you privately connect VPC to AWS services of:
* the same account
* VPC endpoint services of other AWS accounts
* supported AWS Marketplace partner services

Traffic between VPC and the services does not leave AWS; it is via Elastic Network Inteface (ENI) and a private IP.

### PCI DSS compliance 
**PCI DSS** - Payment Card Industry Data Security Standard

## Configuration Options
### VPC
VPC spans all the AZs in a region.
* **Endpoint Services** - create a VPC endpoint to AWS services(such as S3) in the same region.
* **Enable Host Names** - when enabled, EC2 instances launched into this VPC will receive DNS names.
* **Hardware Tenancy** - EC2 instances launched into VPC are run on shared(default) or dedicated hardware.
* To add IPv4 CIDR blocks to VPC, the following rules apply
    * the allowed VPC CIDR block size is between /16 and /28.
    * the range of CIDR blocks withing the same VPC should not overlap.
    * the range of existing CIDR blocks cannot be changed.
    * if classic link is enabled, the you can user address within 10.0.0.0/16 or 10.1.0.0/16 only. 
* To add IPv6 CIDR blocks to VPC, the following rules apply
    * a single IPv6 CIDR block can be associated with VPC with a fixed prefix length of /56.
    
     

### Subnet
Each subnet must reside entirely within one AZ and cannot span AZs.
* **public subnet** - if a subnet has route to IG.
    * to communicate with Internet via IPv4, a EC2 instance must have a public IP or EIP.
    * to communicate with Internet via IPv6, a EC2 instance must have a IPv6 address.
* **private subnet** - if a subnet has not route to IG.
* **VPN-only subnet** - doesn't have route to IG, but its traffic is routed to virtual private gateway. Currently we don't support IPv6 traffic over a site-to-site VPN connection.
* IPv4 subnet
    * the allowed CIDR block size is between /16 and /28.
    * five address in each subnet are reserved:
        * 10.0.0.0 - reserved for network address
        * 10.0.0.1 - reserved by AWS for VPC router
        * 10.0.0.2 - reserved by AWS for DNS. VPC DNS is always the VPC base address plus 2, but AWS also reserve for each subnet base address plus 2.
        * 10.0.0.3 - reserved by AWS for future use.
        * 10.0.0.4 - network broadcast address, because VPC does not support broadcast so reserve it.
* IPv6 subnet
    * a subnet's IPv6 CIDR block uses a fixed prefix length of /64. 
    * the following 5 addresses are reserved by AWS
        * 2001:db8:1234:1a00::
        * 2001:db8:1234:1a00::1
        * 2001:db8:1234:1a00::2
        * 2001:db8:1234:1a00::3
        * 2001:db8:1234:1a00:ffff:ffff:ffff:ffff
        
### Subnet Routing
* Each subnet must be associated with a routing table. 
* Every subnet is automatically associated with the main route table of VPC.

### Internet Gateway
* **VPC ID** - specify VPC the IG is attached to.

### Security
* Security group controls inbound and outbound traffic of EC2 instances
* Network ACL controlls inbound and outbound traffic of subnets
* Flow log can be configured at VPC/subnet/NI level; flow logs are published to CloudWatch logs. 

#### Security Group
Security group is a virtual firewall to control the traffic for its associated EC2 instances.
* **VPC ID** - VPC this security group belongs to.
* **Inbound Rules** - control incoming traffic to EC2 instances.
* **Outbound Rules** - control outgoing traffic from EC2 instances.

{% include links.html %}
