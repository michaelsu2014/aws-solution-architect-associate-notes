# Enterprise identity federation 

In an enterprise identity federation, you can authenticate users in your organization's network, and then provide those users access to AWS without creating new AWS identities for them and requiring them to sign in with a separate user name and password. This is known as the single sign-on (SSO) approach to temporary access.

- Setup a Federation proxy or an Identity provider

- Setup an AWS Security Token Service to generate temporary tokens

- Configure an IAM role and an IAM Policy to access the bucket.

![Alt text](../images/sts-single-signon.png?raw=true)

## Security Token Service (STS)
- Allows to grant limited and temporary access to AWS resources.
- Token is valid for up to one hour (must be refreshed)

- AssumeRole
    - Within your own account: for enhanced security
    - Cross Account Access: assume role in target account to perform actions there
    
![Alt text](../images/sts.png?raw=true)

![Alt text](../images/sts-example.png?raw=true)

https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html    


## Identity Federation in AWS


What?
- Federation lets users outside of AWS to assume user temporary role for accessing AWS resources.
- These users assume identity provided access role.

Why?
- Benefit: Using federation, you don’t need to create IAM users (user management is outside of AWS)

How?
Federations can have many flavors: 
- SAML 2.0

- Custom Identity Broker (Not SAML compatible)

- Web Identity Federation 
    - with Amazon Cognito
    - without Amazon Cognito (not recommended)

- Single Sign On

- Non-SAML with AWSMicrosoftAD

![Alt text](../images/federated-login.png?raw=true)


### SAML 2.0 Federation

What is SAML?
Security Assertion Markup Language (SAML, pronounced SAM-el)[1] is an open standard for exchanging authentication and authorization data between parties, in particular, between an identity provider and a service provider. SAML is an XML-based markup language for security assertions (statements that service providers use to make access-control decisions)


- To integrate Active Directory / ADFS with AWS (or any SAML 2.0) 
- Provides access to AWS Console or CLI (through temporary creds) 
- No need to create an IAM user for each of your employees

- Needs to setup a trust between AWS IAM and SAML (both ways)
- SAML 2.0 enables web-based, cross domain SSO
- Uses the STS API: AssumeRoleWithSAML

Note federation through SAML is the “old way” of doing things
- Amazon Single Sign On (SSO) Federation is the new managed and simpler way

### Custom Identity Broker Application

- Use only if identity provider is **not compatible** with SAML 2.0
    - you need to build a custom app for this.

- The identity broker must determine the appropriate IAM policy 
- Uses the STS API: AssumeRole or GetFederationToken

### Web identity Federation – AssumeRoleWithWebIdentity

Not recommended by AWS – use Cognito instead (**become Cognito allows for anonymous users, data synchronization, MFA**)

With web identity federation, you don't need to create custom sign-in code or manage your own user identities. Instead, users of your app can sign in using a well-known identity provider (IdP) —such as Login with Amazon, Facebook, Google, or any other **OpenID Connect (OIDC)-compatible IdP**, receive an authentication token, and then exchange that token for temporary security credentials in AWS that map to an IAM role with permissions to use the resources in your AWS account.

### AWS Cognito

- Goal:
    - Provide direct access to AWS Resources from the Client Side (mobile, web app) 
    
- Example:
    - provide (temporary) access to write to S3 bucket using Facebook Login

- Problem:
    - We don’t want to create IAM users for our app

- How:
    - Log in to **federated identity provider** – or remain anonymous in Cognito
    - Get temporary AWS credentials back from the Federated Identity Pool
    - These credentials come with a pre-defined IAM policy stating their permissions

![Alt text](../images/cognito.png?raw=true)

### AWS Directory Services

- AWS Managed Microsoft AD
    - Create your own AD in AWS, manage users
    - Establish “trust” connections with your on- premise AD

- AD Connector
    - Directory Gateway (proxy) to redirect to on-proxy premise AD
    - Users are managed on the on-premise AD

- Simple AD
    - AD-compatible managed directory on AWS
    - Cannot be joined with on-premise AD    

![Alt text](../images/AD.png?raw=true)

## AWS Organization

- Global service managing **multiple AWS accounts**
    - The main account is the master account – you can’t change it
    - Member accounts: they can only be part of one organization

- Benefit: Consolidated Billing across all accounts - single payment method
    - Pricing benefits from aggregated usage (volume discount for EC2, S3...) 

![Alt text](../images/org.png?raw=true)

### Service Control Policies (SCP)

- Whitelist or blacklist IAM actions

- Applied at AWS organizations entity (the Root, OU or Account level)

- SCP is applied to all the Users and Roles of the Account, including Root

- SCP must have an explicit Allow (does not allow anything by default)
- Use cases:
    - Restrict access to certain services (for example: can’t use EMR) 
    - Enforce PCI compliance by explicitly disabling services

### AWS Single Sign-On (SSO)
- Centrally manage Single Sign-On to access multiple accounts and 3rd-party business applications.

- Integrated with AWS Organizations
    - Supports SAML 2.0 markup

- Integration with on-premise Active Directory

https://aws.amazon.com/blogs/security/introducing-aws-single-sign-on/

![Alt text](../images/aws-sso.png?raw=true)    
## IAM Advanced
AWS supports **6 types of policies**: identity-based policies, resource-based policies, permissions boundaries, Organizations SCPs, ACLs, and session policies.

### IAM Conditions
![Alt text](../images/conditions.png?raw=true)
![Alt text](../images/conditions2.png?raw=true)

### IAM for S3

- ListBucket permission applies to arn:aws:s3:::test
    - => bucket level permission

- GetObject, PutObject, DeleteObject applies to arn:awn:s3:::test/*
    - => object level permission

### IAM Roles vs Resource Based Policies
- When you assume a role (user, application or service), you give up your original permissions and take the permissions assigned to the role
- When using a resource based policy, the principal doesn’t have to give up his permissions

https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_identity-vs-resource.html
    
### IAM Permission Boundaries

- IAM Permission Boundaries are **only supported for users and roles (not IAM groups)**
- Advanced feature to use a managed policy to set the maximum permissions an IAM entity can get.     
    
### IAM Policy Evaluation Logic

Org SCP > Resourced based Policy > IAM Boundary > Session Policy > identity-based policy    

https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html

![Alt text](../images/policy-evaluation.png?raw=true)

## AWS Resource Access Manager (RAM)

- Share AWS resources that you own with other AWS accounts 
- Share with any account or within your Organization
- Avoid resource duplication!

- Each account...
    - is responsible for its own
    - cannot view, modify or delete other resources in other accounts

- Network is shared so...
    - Anything deployed in the VPC can talk to other resources in the VPC
    - Applications are accessed easily across accounts, using private IP!
    - Security groups from other accounts can be referenced for maximum security

## IAM global service

IAM roles are global service that are available to all regions hence, all you have to do is assign the existing IAM role to the instance in the new region.