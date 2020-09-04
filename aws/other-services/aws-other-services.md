## Opswork
Managed Chef and Puppet for CM

## AWS Control Tower
The easiest way to set up and govern a new, secure multi-account AWS environment


## AWS ParallelCluster
AWS ParallelCluster is an AWS-supported open source cluster management tool that 
- helps you to **deploy and manage High Performance Computing (HPC) clusters** in the AWS Cloud. 


## AWS X-Ray
AWS X-Ray helps developers analyze and debug production, distributed applications, such as those built using a microservices architecture. ... 

- X-Ray provides an end-to-end view of requests as they travel through your application, and **shows a map of your application's underlying components**.

## Amazon MQ

Amazon MQ is a managed **message** **broker service for Apache ActiveMQ** that makes it easy to set up and operate message brokers in the cloud.

- Message brokers allow different software systems-often using different programming languages, and on different platforms-to communicate and exchange information. 

- Connecting your current applications to Amazon MQ is easy because it uses industry-standard APIs and protocols for messaging, including JMS, NMS, AMQP, STOMP, MQTT, and WebSocket.

## EMR (Elastic MapReduce)

Amazon EMR is the industry-leading cloud big data platform for processing vast amounts of data using open source tools such as **Apache Spark**, Apache Hive, Apache HBase, Apache Flink, Apache Hudi, and Presto.

- big data


- Amazon EMR uses Hadoop, an open source framework, to distribute your data and processing **across a resizable cluster of Amazon EC2 instances**.
    - EMR is for launching Hadoop / Spark clusters 

Note: You can access the operating system of these EC2 instances that were created by Amazon EMR.

![Alt text](../images/aws-emr.png)
    
## Athena (雅典娜，智慧，所以是QUERY data IN S3)

- an **interactive query service** that makes it easy to analyze **data directly in Amazon S3 using standard SQL**.

- Athena is **serverless**, so there is no infrastructure to setup or manage, and customers pay only for the queries they run.

- You can use Athena to process logs, perform ad-hoc analysis, and run interactive queries.

![Alt text](../images/athena.png)

## Elasticsearch

Elasticsearch is a search engine based on the Lucene library. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents.
- search with HTTP 
- need to set up a ES cluster

## Amazon Redshift （红色迁移）
Amazon Redshift is a **fully-managed** petabyte-scale cloud based **data warehouse** product designed for large scale **data set storage and analysis.**
- petabyte-scale, very large data, 1m GB,
- data warehouse, data storage and analysis.

- now has the ability to automatically back up your cluster to a second AWS region

- Redshift also provides **columnar storage** 

- this type of database is primarily used for online **analytical** processing (OLAP)
    - Concurrency Scaling is simply an Amazon Redshift feature that automatically and elastically scales query processing power of your Redshift cluster to provide consistently fast performance for hundreds of **concurrent queries**.

https://aws.amazon.com/blogs/aws/automated-cross-region-snapshot-copy-for-amazon-redshift/

- Use Case:
    - analytics with lots of data, can integrate with Kinesis firehose

- enabling **Enhanced VPC routing** on your Amazon Redshift cluster
    - use **VPC flow logs** to monitor all the COPY and UNLOAD traffic of your Redshift cluster that moves in and out of your VPC. 

Note: Audit Logging feature is primarily used to get the information about the connection, queries, and user activities in your Redshift cluster.

###  Amazon Redshift Spectrum
Using Amazon Redshift Spectrum, you can **efficiently query and retrieve structured and semi-structured data from files in Amazon S3**
 - without having to load the data into Amazon Redshift tables.

## Amazon Neptune （海王星，知道很多关系）

Amazon Neptune is a fast, reliable, **fully managed** **graph** database service that makes it easy to build and run applications that work with highly connected datasets. The core of Amazon Neptune is a purpose-built, high-performance graph database engine optimized for storing billions of relationships and querying the graph with milliseconds latency.
- graph DB, meaning to query something e.g. complicated queries such as "What are the number of likes on the videos that have been posted by the friends of Mike?"

## AWS Glue
- Glue AWS Glue is a fully managed **extract, transform, and load (ETL)** service that makes it easy for customers to prepare and **load their data for analytics**. You can create and run an ETL job with a few clicks in the AWS Management Console.

- what is the use case?
    - prepare and load their data for analytics

## Amazon WorkDocs

Amazon WorkDocs is simply a fully managed, secure content creation, storage, and collaboration service. 

With Amazon WorkDocs, **you can easily create, edit, and share content.** 

And because it’s stored centrally on AWS, you can access it from anywhere on any device.
    
## Amazon WorkSpaces
Amazon WorkSpaces is a managed, **secure cloud desktop service**. 
- A desktop experience in the cloud that can be accessed from any connected device.

- You can use Amazon WorkSpaces to provision either Windows or Linux desktops in just a few minutes and quickly scale to provide thousands of desktops to workers across the globe. 
- You can pay either monthly or hourly, just for the WorkSpaces you launch, which helps you save money when compared to traditional desktops and on-premises VDI solutions.

- Use Case:
    - it helps mobile and remote employees access the applications users need by delivering a cloud desktop accessible anywhere with an internet connection using any supported device.

## AWS AppSync

- Real time data sync using **GraphQL** for mobile app, online, and offline

## AWS ECS – Elastic Container Service
- ECS, Elastic Container Service
    - run Docker in EC2 machines

![Alt text](../images/ecs-diagram.png?raw=true)

- ECS is complicated, and made of:
    - “ECS Core”: Running ECS on user-provisioned EC2 instances
    - **Fargate**: Running ECS tasks on **AWS-provisioned compute** (serverless) 
        - With Fargate, it’s all **Serverless**!
        - We don’t provision EC2 instances. We just create task definitions, and AWS will run our containers for us 
        - To scale, just increase the task number. Simple! No more EC2 J
    - EKS: Running ECS on AWS-powered Kubernetes (running on EC2)
    - ECR: Docker Container Registry hosted by AWS    

    ![Alt text](../images/ecr-cd.png?raw=true)

- IAM security and roles at the ECS task level    

![Alt text](../images/ecs-iam.png?raw=true)

### ECS Use Case

- Run microservices
    - Ability to run multiple docker containers on the same machine 
    - Easy service discovery features to enhance communication
    - Direct integration with Application Load Balancers
        - **"port mapping"**: This allows you to run multiple instances of the same application on the same EC2 machine
        - Use cases:
          - Increased resiliency even if running on one EC2 instance
          - Maximize utilization of CPU / cores
          - Ability to perform rolling upgrades without impacting application uptime
    ![Alt text](../images/ecs-alb.png?raw=true)
    - Auto scaling capability

- Run batch processing / scheduled tasks
    - Schedule ECS containers to run on On-demand / Reserved / Spot instances

- Migrate applications to the cloud
    - Dockerize legacy applications running on premise
    - Move Docker containers to run on ECS

### AWS SWF vs Step Functions

#### Step
AWS Step Functions makes it easy to coordinate the components of distributed applications as a series of steps in a visual workflow. 

- You need to have a service which can coordinate multiple AWS services into serverless workflows.

- You build applications from individual components that each perform a discrete function, or task, allowing you to scale and change applications quickly.

It will orchestrate different aws services together to do something interesting.. :)

#### SWF

Amazon Simple Workflow Service (Amazon SWF) is a workflow service for building scalable, resilient applications. With Amazon SWF, you can build many kinds of applications as workflows. 

- It ensures that a task is **never duplicated** and is assigned only once.
These facilities enable you to coordinate your workflow without worrying about duplicate, lost, or conflicting tasks.

#### Differences 
- Step Functions is a **managed** service, so users don't have to deploy or maintain any infrastructure for either the workflow management or the tasks themselves. 

- SWF also manages workflow state in the cloud. However, **unlike Step Functions, a user has to manage the infrastructure that runs the workflow logic and tasks.** 
    
### Sample Questions

#### How to ensure DB credentials are secure and cannot be viewed in plaintext on the cluster itself?

A:  the recommended way to secure sensitive data is either through the use of **Secrets Manager** or **Systems Manager Parameter Store**.

- Use the AWS Systems Manager Parameter Store to keep the database credentials and then encrypt them using AWS KMS. 

- Create an **IAM Role** for your Amazon ECS task execution role (taskRoleArn) and reference it with your task definition, which allows access to both KMS and the Parameter Store. 
    - Note not resourced based policy.
- Within your container definition, specify secrets with the name of the environment variable to set in the container and the full ARN of the Systems Manager Parameter Store parameter containing the sensitive data to present to the container.    

## Access to underlying system

Heres the list based on Linux Academy SysOps Course:

Amazon EMR - We can spin up number of EC2 instance to load our data and perform Analysis. So we have access to those EC2 instances and hence have access to the underlying OS

Amazon EC2 - We can install software or modify core OS settings and run Utilities

Amazon ECS - Gives us ability to connect to Container Instances to perform Admin Activities

Amazon Elastic Beanstalk - Lets you deploy and manage Applications, which will spin Instances and hence you get access to underlying OS

Amazon OpsWorks - Gives you configuration management capabilities so you can do package installation etc

On the contrary, Services like Amazon RDS, DynamoDB, Lambda and ElastiCache give absolutely no Access to underlying OS, since they are all Managed Services.
