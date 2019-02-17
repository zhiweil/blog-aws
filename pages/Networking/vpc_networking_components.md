---
title: VPC Networking Components
tags: [Networking]
keywords: AWS VPC
last_updated: February 16, 2019
summary: "The key components of AWS VPC networking"
sidebar: mydoc_sidebar
permalink: vpc_networking_components.html
folder: Networking
---

## Elastic Network Interface (ENI)
A network interface's attributes follow it as it is attached or detached from an instance
and reattached to another instance. When you move a network interface from one instance to another,
network traffic is redirected to the new instance.

All EC2 instances of VPC have a primary ENI (eth0), which can be detached.

Besides IP addresses and MAC address, an ENI can be assigned with the following stuff:
* one or more security groups
* a source/destination check flag

## Routing Table
A subnet must be associated with one routing table; a routing table can be associated with multiple subnets.
* VPC comes with a default router and a main modifiable routing table. You cannot delete main routing table.
* You can specify a custom routing table as main routing table.
* A new subnet is associated with main routing table implicitly; until it is associated with a custom subnet explicitly.

### Routing priority
**Longest prefix (most specific) match**

## Internet Gateway
To enable Internet access:
* attach IG to VPC
* add route to routing table pointing to IG
* assign public IP to EC2 instances
* configure network ACL and security group to enable internet flow

## Egress-Only Internet Gateway
* Allows IPv6 outbound communication from VPC to Internet. Egress-only IG is for IPv6 only.
* Egress-only IG is stateful, which means that when you enable outbound flow the corresponding inbound flow
is enabled automatically
* Egress-only IG does not support IPv4

## NAT
**NAT cannot be used with IPv6.**

### NAT Gateway
* NAT gateway is a managed service
* when creating a NAT gateway, you must
    * specify a public subent
    * specify a EIP
    * to achieve better availability in multiple AZs, you'll create multiple NAT gateways in each AZ, and configure resources to use the NAT gateway in the same AZ.
* NAT gateway supports 5 Gbps, can scale upto 45 Gbps automatically. If you require more, you'll need to split your VPC into more subnets and create more NAT gateways.
* EIP cannot be de-associated with NAT gateway after it is created. To use a different EIP, you'll need to create a new NAT gateway.
* NAT gateway supports TCP, UDP and ICMP.
* NAT gateway cannot be accessed by ClassicLink connection.
* NAT gateway can create upto 55000 concurrent connections to the single destination (per minutes); 900 concurrent connection per second to a single connection.
* the "ErrorPortAllocation" error can be monitored in CloudWatch, which indicates that the number of connections might be too many.(
1-minute frequence and upto 15 months persistence)

A NAT gateway **CANNOT** send traffic over
* VPC endpoint
* AWS site-to-site VPN connection
* AWS direct connect
* VPC peering connection

Best practice for sending traffic to S3 and DynamoDB
**Create a NAt gateway endpoint, route traffic via endpoint (no extra charge) instead of NAt gateway**
 
#### Monitoring by Cloudwatch Metrics
* ActiveConnectionCount
* BytesInFromDestination (from outside VPC)
* BytesInFromSource (from inside VPC)
* BytesOutToDestination
* BytesOutToSource
* ConnectionAttemptsCount
* ConnectionEstablishedCount
* ErrorPortAllocation(**important metric indicating that NAT gateway is too busy**)
* IdleTimeoutCount (**connections not closed gracefully will be transitioned from active to idle**)
* PacketsDropCount (**indicating an ongoing transient issue**)
* PacketsInFromDestination
* PacketsInFromSource
* PacketsOutToDestination
* PacketsOutToSource

There is one supported dimension:
* NatGatewayId

### NAT Instance
**NAT instance is for IPv4 only.** Amazon provides EC2 AMI that supports NAT. There is string **amzn-ami-vpc-nat** 
in their names.
* IPv4 forwarding is enabled and ICMP redirects are disabled in /etc/sysctl.d/10-nat-
settings.conf
* A script located at /usr/sbin/configure-pat.sh runs at startup and configures iptables IP
masquerading.
* the source/destination check option **MUSt** be disabled! If NAT instance has more ENIs, should 
do this for all of them.
* NAT instance is simply an EC2 instance, which limitation is up to instance type chosen.

### NAT Gateway and NAT instance

Attributes|NAT Gateway|NAT Instance| 
----------|-----------|------------| 
Availability|AWS managed. Very high.|Managed by User. Need a script to fail over.| 
Bandwidth|upto 45 Gbps|depending on EC2 instance type| 
Maintenance | Amazon | You |
Performance | Amazon | You |
Cost | changed per NAT gateway, data flow and etc.| by EC2 |
Type & Size | you have no choice | you choose by EC2 |
Public IP | EIP assigned at creation | EIP or public IP |
Private IP | assigned when created in subnet | assigned when created in subnet |
Security Group| No | Yes |
Network ACL | control at subnet level | control at subnet level |
Port Forwarding| not supported | can be configured |
Bastion Server | not supported | can be configured as bastion server |
Timeout behaviour | RST packet is sent to source | FIN packet is sent to source|
IP Fragmentation| support UDP, not TCP and ICMP | support TCP, UDP and ICMP|




