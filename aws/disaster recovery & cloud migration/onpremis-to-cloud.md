    
## DMS - Database Migration Service

Quickly and securely migrate on-premise databases to AWS, resilient, self healing   
 
- AWS Schema ConversionTool (SCT)
    - **You do not need to use SCT if you are migrating the same DB engine** 
        - Ex: On-Premise PostgreSQL => RDS PostgreSQL
        - The DB engine is still PostgreSQL (RDS is the platform)

![Alt text](../images/dms.png?raw=true)

![Alt text](../images/sct.png?raw=true)        

## On-Premise strategy with AWS

- Ability to download Amazon Linux 2 AMI as a VM (.iso format)
    - VMWare, KVM,VirtualBox (Oracle VM), Microsoft Hyper-V

- VM Import / Export
    - Migrate existing applications into EC2
    - Create a DR repository strategy for your on-premiseVMs
    - Can export back the VMs from EC2 to on-premise

- AWS Application Discovery Service
    - Gather information about your on-premise servers to plan a migration
    - Server utilization and dependency mappings
    - Track with AWS Migration Hub

- AWS Database Migration Service (DMS)
    - replicate On-premise => AWS , AWS => AWS, AWS => On-premise
    - Works with various database technologies (Oracle, MySQL, DynamoDB, etc..)

- AWS Server Migration Service (SMS)
    - Incremental replication of on-premise **live** servers to AWS

## AWS DataSync

- Move large amount of data from on- premise to AWS
- Can synchronize to: Amazon S3, Amazon EFS, Amazon FSx for Windows

- need to install DataSync agent on premise to work

https://docs.aws.amazon.com/datasync/latest/userguide/how-datasync-works.html

## How to bring onpremis IP to AWS?

- Bring your own IP addresses (BYOIP) in Amazon EC2

Solution: create **A Route Origin Authorization (ROA)** 
- You can simply create a Route Origin Authorization (ROA) then once done, provision and advertise your whitelisted IP address range to your AWS account.