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