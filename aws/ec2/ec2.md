
## Instance Types

- R: applications that needs a lot of RAM – in-memory caches
- C: applications that needs good CPU – compute / databases

- M: applications that are balanced (think “medium”) – general / web app • I: applications that need good local I/O (instance storage) – databases

- G: applications that need a GPU – video rendering / machine learning

- T2 / T3: burstable instances (up to a capacity) 
    - T2 / T3 - unlimited: unlimited burst

## Instance Launch Types

- On Demand Instances: short workload, predictable pricing
    - **Has the highest cost** but no upfront payment
    - Recommended for short-term and un-interrupted workloads, where you can't predict how the application will behave.

- Reserved: (MINIMUM 1 year)
    - Reserved Instances: long workloads
    - Convertible Reserved Instances: long workloads with flexible instances
    - Scheduled Reserved Instances: example – everyThursday between 3 and 6 pm
    
    - Recommended for steady state usage applications (think database)
    - **Up to 75% discount compared to On-demand**
    - Pay upfront for what you use with long term commitment

- Spot Instances: short workloads, for cheap, can lose instances (less reliable) 
    - **The MOST cost-efficient instances in AWS** Can get a discount of up to 90% compared to On-demand
    
    - Useful for workloads that are resilient to failure 
        - Batch jobs
        - Data analysis
        - Image processing •...
    - Not great for critical jobs or databases
        - interruption means instance termination
    - Great combo: Reserved Instances for baseline + On-Demand & Spot for peaks
    
    - Spot Fleet request: we **can select instance type in advance for Spot instances**

- Dedicated Instances: no other customers will share your hardware
    - No control over instance placement (can move hardware after Stop / Start)

- Dedicated Hosts: book an entire physical server, control instance placement
    - Physical dedicated EC2 server for your use
    - Useful for software that have complicated licensing model (BYOL – Bring Your Own License)
    - Or for companies that have strong regulatory or compliance needs

## Security Group

- Act like a virtual firewall that controls the traffic

- by default allow all outbound traffic
- rules are always permissive, meaning you cannot deny access
- sg are **stateful**, which means you do not need to explicitly add allow for outbound if the inbound is allowed
- when evaluating rules, evaluate all the rules from all the security groups that are associated with the instance.

## Elastic IP
- For that Elastic IP to be attached to our EC2 instance, we must use 
    - Use an EC2 user data script, 
    - must have the correct IAM permissions to perform the API call, so we need an EC2 instance role.
    
- we can use Elastic IP to one EC2, because we want to keep cost down and also there is only 1 EC2 to connect, e.g. monolith app.    

## EC2 Instance Hibernate

With it, EC2 instance can keep its data in EBS, why
- Hibernation saves the contents from the **instance memory (RAM)** to your Amazon EBS root volume. 
- AWS then persists the instance's Amazon EBS root volume and any attached Amazon EBS data volumes.
- When you start your instance: The Amazon EBS root volume is restored to its previous state The RAM contents are reloaded The processes that were previously running on the instance are resumed Previously attached data volumes are reattached and the instance retains its instance ID

## Elastic Fabric Adapter (EFA)
The Elastic Fabric Adapter (EFA) is a network interface for Amazon EC2 instances that enables customers to 
- **run HPC applications requiring high levels of inter-instance communications**, like computational fluid dynamics, weather modeling, and reservoir simulation, at scale on AWS.

- must **include an OS-bypass functionality**

## Elastic Network Adapters (ENAs)

Elastic Network Adapters (ENAs) provide traditional IP networking features that are required to support VPC networking. EFAs provide all of the same traditional IP networking features as ENAs, and they also support OS-bypass capabilities. OS-bypass enables HPC and machine learning applications to bypass the operating system kernel and to communicate directly with the EFA device.

### EFA vs ENA

EFA brings the scalability, flexibility, and elasticity of the cloud to tightly-coupled HPC (High-performance computing)applications. ... An EFA is an Elastic Network Adapter (ENA) with added capabilities. It provides all of the functionality of an ENA, **with additional OS-bypass functionality**.

ENA:
- Amazon EC2 provides enhanced networking capabilities through the Elastic Network Adapter (ENA). It supports network speeds of up to 100 Gbps for supported instance types. Elastic Network Adapters (ENAs) provide traditional IP networking features that are required to support VPC networking.

EFA:
- An Elastic Fabric Adapter (EFA) is simply an Elastic Network Adapter (ENA) with added capabilities. It provides all of the functionality of an ENA, **with additional OS-bypass functionality**.
- OS-bypass is an access model that allows HPC and machine learning applications to communicate directly with the network interface hardware to provide low-latency, reliable transport functionality.
  - **The OS-bypass is not supported on Windows instances.** 

  

## Migration

- Migrate on-premise app with IP address to AWS
    - A **Route Origin Authorization (ROA)**, not useing Elastic IP
    
## vCPU-based On-Demand Instance limit

![Alt text](../images/ec2-vcpu.png?raw=true)    

## Billing

- Reserved Instances that applied to terminated instances are still billed until the end of their term according to their payment option. 

- when the instance state is stopping, you will not billed if it is preparing to stop however, you will still be billed if it is **just preparing to hibernate**.

## Monitor EC2 instance using CloudWatch

if the metric is not in the default, there are 2 ways that send the data to CloudWatch
- Use monitoring script 
- Use a CloudWatch agent 

- by default, CloudWatch **doesn't monitor** **memory usage** but only the CPU utilization, Network utilization, Disk performance and Disk Reads/Writes.
    - CloudWatch does not monitor EC2 memory usage as well as disk space utilization.
    
- The Amazon CloudWatch **Monitoring Scripts** for Amazon Elastic Compute Cloud (Amazon EC2) Linux-based instances demonstrate how to produce and consume Amazon CloudWatch custom metrics. These sample Perl scripts comprise a fully functional example that reports memory, swap, and disk space utilization metrics for a Linux instance.    

## Config EC2 without via Remote Desktop Protocol (RDP) for windows or SSH

**AWS Systems Manager Run Command** lets you remotely and securely manage the configuration of your managed instances.

Note: 
-  **Session Manager** is simply a capability that lets you manage your Amazon EC2 instances through an interactive one-click browser-based shell or through the AWS CLI.

## Reasons of unable SSH the instance

You might be unable to log into an EC2 instance if:

- You're using an SSH private key but the corresponding public key is not in the authorized_keys file.

- You don't have permissions for your authorized_keys file.

- You don't have permissions for the .ssh folder.

- Your authorized_keys file or .ssh folder isn't named correctly.

- Your authorized_keys file or .ssh folder was deleted.

- Your instance was launched without a key, or it was launched with an incorrect key.

To connect to your EC2 instance after receiving the error "Server refused our key," you can update the instance's user data to append the specified SSH public key to the authorized_keys file, which sets the appropriate ownership and file permissions for the SSH directory and files contained in it.



Reference:

https://aws.amazon.com/premiumsupport/knowledge-center/ec2-server-refused-our-key/

## Elastic ip

An Elastic IP address doesn’t incur charges as long as the following conditions are true:

- The Elastic IP address is associated with an Amazon EC2 instance.
- The instance associated with the Elastic IP address is running.
- The instance has only one Elastic IP address attached to it.

If you don't use elastic ip, you are going to pay for it.

## Remote Desktop Protocol (3389)