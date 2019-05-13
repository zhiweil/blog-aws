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
    
    
## Corporate Identity Federation
* Corporate identity federation allows you to use existing IdP to access AWS resources.
* Identity stores can be AWS directory services, any SAML compatible providers (AD or Shibboleth) or a custom
    federation proxy.
* Works in a similar way to delegation - uses the Role architecture - TRUST and ACCESS policies to control and allow
    permissions.
* Temporary access is provided by STS and access is obtained by STS:GetFederationToken or STS:AssumeRole.

### How it works
* STS:AssumeRole - STS provides you with session credentials: AKID, secret access key, session token and expiration.
* Expiration values are min/max/Default (depending on the type of services or type of calls). 
    * AssumeRole: 15min, 1 hour, 1 hour (min/max/default).
    * GetFederationToken: 15min, 36 hours and 12 hours.
* Within AWS, there is a concept of "identity provider" which is an IAM object which holds the configuration information
    about the external identity provider.
    * we generally map external groups (in such as AD services) in external ID providers with roles in AWS. 
    
### Why Identity Federation
* It allows seperation of responsibilities. Your organization might have a dedicated identity team. 
* You have definitive source of identities within your business, eg. HR entries and exit processes are tied to this. 
* You minimize the admin overhead regarding identity management in the business. 
* You reduce the number of identities your staff need to remember and manage - reduced attack footprint. 

### Example:  Custom Proxy - Console - AssumeRole
This is similar to identity delegation. This mechanism is for user access.

1. Corporate user browses corporate federation proxy (custom proxy)
2. Federation proxy authenticates the user with corporate AD
3. If succees. AD returns a list of groups of the user in AD
4. Federation proxy asks "list roles" to AWS IAM
5. AWS IAM returns a list roles the user belongs to. At this stage, federation proxy presents a list of 
    roles for user to choose.
6. User chooses a role
7. STS:AssumeRole
8. STS responds with credentials
9. Generate URL and redirect
10. AWS console access

### Example: Custom proxy - API - GetFederationToken
This mechanism is for application access.

1. User application requests a session from federation proxy
2. Federation proxy authenticates you with AD
3. AD returns a list of entitlements which your application has access to. These equates the roles on AWS side.
4. Federation proxy invokes STS:GetFederationToken
5. STS responds with credentials
6. Federation proxy returns session to user application. 
7. User application invokes APIs on AWS

The security issues of this approach: 
* user application needs all the IAM permissions to run the application.
* Get federation token does not support MFA

The common issue of both approaches above: bot approaches needs IAM permissions to function, which must be
managed securely.

### Example: SAML - Console - AssumeRoleWithSAML
1. Corporate user browses to the identity provider (such as ADFS server)
2. Identity provider authenticates the user with AD
3. If succeeds, identity provider returns SAML token to corporate user
4. User uses the returned SAML token to authenticates itself with AWS sign-in endpoint
5. Internally, AWS sign-in endpoint calls STS:AssumeRoleWithSAML
6. STS responds with credentials
7. AWS sign-in endpoint construct and returns console URL
8. Corporate user is redirected to AWS console

The benefits of this approach:
* You don't need to maintain custom dedicated proxies. 
* The proxy doesn't need to maintain IAM permissions. 

## Web Identity Federation
* Web identity federation allows a trusted third party to authenticate users. 
* It avoids us from having to create and manage users in IAM
* It avoids users from having to remember multiple credentials (SSO).
* It simplifies access control via roles. 
* It improves security, no permanent credentials stored in your applications.
* Using web identity federation and Cognito also provides user state syncing. 