## Encryption Type

You can secure the privacy of your data in AWS, both at rest and in-transit, through encryption. 
- If your data is stored in EBS Volumes, you can enable EBS Encryption 
- If it is stored on Amazon S3, you can enable client-side and server-side encryption.

### Encryption in flight (SSL)
- Data is encrypted before sending and decrypted after receiving
- SSL certificates help with encryption (HTTPS)
- Encryption in flight ensures no MITM (man in the middle attack) can happen

![Alt text](../images/sts.png?raw=true)

### Server side encryption at rest
the server will encrypt the data for us. We don't need to encrypt it beforehand

Both encryption and decryption happen in the server
- Data is encrypted after being received by the server
- Data is decrypted before being sent
- It is stored in an encrypted form thanks to a key (usually a data key)
- The encryption / decryption keys must be managed somewhere and the server must have access to it

https://docs.aws.amazon.com/kms/latest/developerguide/services-s3.html


### Client side encryption

With client side encryption, the server does not need to know any information about the encryption being used, as the server won't perform any encryption or decryption tasks

- Data is encrypted by the client and never decrypted by the server 
- Data will be decrypted by a receiving client
- The server should not be able to decrypt the data
- Could leverage Envelope Encryption

To enable client-side encryption, you have the following options:

- Use a customer master key (CMK) stored in AWS Key Management Service (AWS KMS).

- Use a master key that you store within your application.

https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingClientSideEncryption.html

## AWS KMS (Key Management Service)

What?
- AWS manages keys for us (fully managed)
- Easily create keys and control encryption across AWS
- KMS is a secure and resilient service that uses FIPS 140-2 validated hardware security modules to isolate and protect your keys.

How?
- your keys never leave KMS
- centrally manage and securely store keys.
- encrypt/decrypt data to KMS

### KMS Basic

- Anytime you need to share sensitive information... use KMS 
    - Database passwords
    - Credentials to external service 
    - Private Key of SSL certificates

-The value in KMS is that the CMK used to encrypt data can never be retrieved by the user, and the CMK can be rotated for extra security

- KMS can only help in encrypting up to 4KB of data per call
    - If data > 4 KB, use envelope encryption

### Customer Master Key (CMK)Types

Symmetric (**AES-256**, meaning encryption std or SSE-S3, meaning S3-managed keys)
- First offering of KMS, **single encryption key that is used to Encrypt and Decrypt**
- AWS services that are integrated with KMS use Symmetric CMKs
- Necessary for envelope encryption
- You never get access to the Key unencrypted (must call KMS API to use)

Asymmetric (RSA & ECC key pairs)
- **Public (Encrypt) and Private Key (Decrypt) pair**
- Used for Encrypt/Decrypt, or Sign/Verify operations
- The public key is downloadable, but you access the Private Key unencrypted
- Use case: 
    - **encryption outside of AWS by users who can’t call the KMS API**

Three types of Customer Master Keys (CMK):
- AWS Managed Service Default CMK: free
- User Keys created in KMS: $1 / month
- User Keys imported (must be 256-bit symmetric key): $1 / month

pay for API call to KMS ($0.03 / 10000 calls)

### Key Policies

- Control access to KMS keys, “similar” to S3 bucket policies
- Difference: you cannot control access without them

- Default KMS Key Policy:
    - Created if you don’t provide a specific KMS Key Policy
    - Complete access to the key to the root user = entire AWS account 
    - Gives access to the IAM policies to the KMS key

- Custom KMS Key Policy:
    - Define users, roles that can access the KMS key
    - Define who can administer the key
    - Useful for cross-account access of your KMS key

### CloudHSM

What?

- Managed hardware security module (HSM) in the AWS Cloud

- AWS provisions encryption hardware
    - HSM = Hardware Security Module
    - you **manage your own encryption keys entirely** (dedicated hardware) using **FIPS 140-2 Level 3 validated HSM**s.

How it works?

- AWS CloudHSM runs in your own (VPC), so that you can easily use your HSMs with applications that run on your Amazon EC2 instances.

- CloudHSM clusters are spread across Multi AZ - must setup
- Must use CloudHSM Client software

- Redshift supports CloudHSM for db encryption and key management   

- No free tier

Why?
- AWS CloudHSM automates time-consuming HSM administrative tasks, such as hardware provisioning, software patching, high availability, and backups. 
- seems it is something for compliance

https://console.aws.amazon.com/cloudhsm/home?region=us-east-1#/home
    
## System Manager - Parameter Store

### System Manager
What is it?
- System Manager
    - many things under this umbrella
        - ops, cloudwatch dashboard
        - app manager, parameter store
        - instances, complainces, session, state, etc

How it works?

![Alt_text](../images/sm-how.png)

### Parameter Store
- SSM Parameter Store has **versioning** and **audit** of values built-in directly - it can check values over time

- Secure storage for configuration and secrets 
- Optional Seamless Encryption using KMS
- Serverless, scalable, durable, easy SDK
- Version tracking of configurations / secrets

- Configuration management using path & IAM 
- Notifications with CloudWatch Events
- Integration with CloudFormation 

Note: you must rotate the secrets yourself, it doesn't have an automatic capability for this.

Hands on lambda + System manager 

https://www.udemy.com/course/aws-certified-solutions-architect-associate-saa-c02/learn/lecture/13528522#overview

## Secrets Manager

What is it?
- Easily rotate, manage, and retrieve secrets throughout **lifecycle**

    The service enables you to easily rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle.

- Newer service, meant for storing secrets

- Difference from SSM: 
    - Capability to **force rotation of secrets every X days** (rotate)
    - focus on secret (manage/retrieve)

How it works?

To retrieve secrets, you simply replace hardcoded secrets in applications with **a call to Secrets Manager APIs**, eliminating the need to expose plaintext secrets.
    
- Automate generation of secrets on rotation (uses Lambda)
- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora) 
- Secrets are encrypted using KMS
- Mostly meant for RDS integration 

Why?
- it used to hard to rotate credentials, because they are embedded in the app.
- it is harder if more apps with shared credentials. If missed updating any, the app failed.

- how Secrets Manager helps?
    - Secrets Manager enables you to replace hardcoded credentials in your code, including passwords, with an API call to Secrets Manager to retrieve the secret programmatically. This helps ensure the secret can't be compromised by someone examining your code, because the secret no longer exists in the code. 
    
https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html?icmpid=docs_asm_console    