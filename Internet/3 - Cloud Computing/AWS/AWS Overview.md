---
sticker: emoji//2601-fe0f
---
# [[OSI (Open Systems Interconnection) model]]
Here it involves the 7 layers of Internet.
1. **Physical Layer (OSI Layer 1)**: AWS Direct Connect, establishes a dedicated network connection between AWS and an on-premises data center, bypassing the public internet.
2. **Data Link Layer (OSI Layer 2)**: N/A
3. **Network Layer (OSI Layer 3)**: 
	- VPC: provide a logically isolated section to launch AWS resources in a virtual network.
	- Route 53: DNS web service for routing end users to Internet applications.
4. **Transport Layer (OSI Layer 4)**:
    - Global Accelerator: Improves availability and performance of apps with local and global traffic management.
	- Network Load Balancer: Distributes incoming application traffic across multiple targets, such as EC2 instances.
5. **Session Layer (OSI Layer 5)**: N/A
6. **Presentation Layer (OSI Layer 6)**: N/A
7. **Application Layer (OSI Layer 7)**:
    - Compute Services:
	    - EC2: Provides resizable compute capacity in the cloud.
	    - Lambda: Runs code without provisioning or managing servers.
	- Storage Services:
	    - S3: Object storage service designed to store and retrieve any amount of data.
	    - EBS: Provides block-level storage volumes for use with EC2 instances.
	- Database Services:
	    - RDS: Managed relational database service supporting various database engines like MySQL, PostgreSQL, etc.
	    - DynamoDB: Fully managed NoSQL database service.
	- Networking Services:
	    - API Gateway: Fully managed service for creating, publishing, maintaining, monitoring, and securing APIs at any scale.
	    - CloudFront: CDN for fast and secure delivery of data, videos, apps, and APIs.
	    - ELB: Automatically distributes incoming application traffic across multiple targets.
	    - Route 53: Scalable DNS web service for routing end users to Internet applications.


Your application running on a fleet of EC2 instances managed by an Auto Scaling Group behind an Application Load Balancer. Users have to constantly log back in and you don't want to enable Sticky Sessions on your ALB as fear it will overload some EC2 instances. What should you do?
- Storing Session Data in ElastiCache is a common pattern to ensuring different EC2 instances can retrieve user's state if needed.

Aurora Global Databases allows you to have an Aurora Replica in another AWS Region, with up to 5 secondary regions.

take a snapshot
restore to point in time
what is the difference then?


### DynamoDB
A NoSQL database service for unstructured data, like a magical notebook that instantly stores and retrieves notes no matter how many you have.


## 4. Keep the remote computer safe (security and identity services)
### IAM: Identity and Access Management
A security system that ensures only the people you've given keys to can enter the house or specific rooms (services) you've designated.
### Security Groups
Like setting up a security system for your home, deciding who can knock on the door (access your instances) and who can't.
- Another layer of abstraction over IP so that don't need to think about IPs when using groups.
- SSH


## 5. Connect Computer to the Internet (networking and content delivery)

### ASG: Auto Scaling Group
The Auto Scaling Group can't go over the maximum capacity (you configured) during scale-out events.
When an EC2 instance fails the ALB Health Checks, it is marked unhealthy and will be terminated while the ASG launches a new EC2 instance.
There's no CloudWatch Metric for "requests per minute" for backend-to-database connections. You need to create a CloudWatch Custom Metric, then create a CloudWatch Alarm.

## 9. Building and Deploying Application (Developer Tools)

### CodeDeploy, CodeBuild, CodePipeline
These services automate code deployment, building, and integration process, akin to having a team of robots that continuously improve and fix your house without you lifting a finger.

## Serverless Overviews from a Solution Architect Perspective
Serverless: don't need to manage/provision servers anymore, just deploy code and functions.
- in AWS
	- Lambda
	- DynamoDB
	- Cognito
	- API Gateway
	- S3
	- SNS & SQS
	- Kinesis Data Firehose
	- Aurora Serverless
	- Step Functions
	- Fargate
### Lambda
For tasks that don't need a dedicated computer running all the time. It's like having robots wake up, perform a task quickly, and go back to sleep without worrying about the electricity bill.

### API Gateway
A centralized server that manages, secures, and optimizes communication between clients and backend services in a microservices architecture.

## Quiz

You are running a high-performance database that requires an IOPS of 310,000 for its underlying storage. What do you recommend?

Use an EC2 instance store

You can run a database on an EC2 instance that uses an Instance Store, but you'll have a problem that the data will be lost if the EC2 instance is stopped (it can be restarted without problems). One solution is to set up a replication mechanism on another EC2 instance with an Instance Store to have a standby copy. Another solution is to set up backup mechanisms for your data. It's all up to you how you want to set up your architecture to validate your requirements. In this use case, it's around IOPS, so we have to choose an EC2 Instance Store.







List of Ports to be familiar with

**Be able to differentiate between an Important (HTTPS - port 443) and a database port (PostgreSQL - port 5432)** 
**Important ports:**
- FTP: 21
- SSH: 22
- SFTP: 22 (same as SSH)
- HTTP: 80
- HTTPS: 443
    
**vs RDS Databases ports:**
- PostgreSQL: 5432
- MySQL: 3306
- Oracle RDS: 1521
- MSSQL Server: 1433
- MariaDB: 3306 (same as MySQL)
- Aurora: 5432 (if PostgreSQL compatible) or 3306 (if MySQL compatible)


## Classic Solutions Architecture
How these technologies fit together?

### Whatisthetime.com
[[State]] Apps
