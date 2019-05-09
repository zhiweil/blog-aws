---
title: Security, Governance and Validation
tags: [devops-exam]
keywords: AWS security, governance and validation
last_updated: May 7, 2019
summary: "Security, Governance and Validation"
sidebar: mydoc_sidebar
permalink: devops_security_governance_validation.html
folder: devops
---
## Identitiy Federation & Delegation
* In AWS, IAM is used as IdP to control access to your AWS resources.
* You can allow users from other AWS account to access your AWS resources directly. This is called
    **Delegation**. This is still the same IdP.
* Federation - allows users from external IdPs to access your account. There are two types of federations:
    * Corporate/Enterprise Identity Federation, sources include AD and LDAP. You can use custom federation proxy, SAML or AWS Directory service.
    * Web or social Identity federation - you would trust other IdPs such as Amazon, Google, Twitter and OpenID Connect.
        This kind of federation is used when you want to give an application or user access (via web or serverless
        application) to your AWS resources. AWS has a service called Cognito to handle such services.
        
There are two concepts that are worth metioning for federation and delegation: role and session.

### Role
A role is an object which contains two policies:
* Trust policy: who is granted to access resources.
* Access policy: what resources are granted for access.

The following example trust the root user of account 1111111 to assume role.
```json
{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::1111111:root"
  },
  "Action": "sts:AssumeRole"
}
```

The following example is a full access policy.
```json
{
  "Effect": "Allow",
  "Resources": "*",
  "Action": "*"
}
```

### Session
* A session is a set of temporary credentials. 
* An access and secrete access key with expiration.
* You obtain them the Security Token Service (STS).
* You obtain them via the following API calls:
    * STS:AssumeRole
    * STS:AssumeRoleWithSAML
    * STS:AssumeRoleWithWebIdentity
* It may or may not involve Cognito - depending on if it is web federation.

The basic process of assuming role from another account is:
* A user from account A authenticate itself with account B by its credentials, Which allows very limited access to account B.
* The user assume another role which is granted much more powerful access to account B.

### Key points
* It is important to realize that roles are used throughout AWS to grant acccess to objects. For example:
    * EC2 is assigned a instance role
    * Lambda allows you to grant a role to lambda function.
* What happens at background is:
    * The service (EC2 or Lambda) is constantly using STS:AssumeRole, obtaining credentials on your behalf and
        making them available to instance or function via meta-data. 
    * The service auto refreshes the session and the credentials on your behalf. 
    
    