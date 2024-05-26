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


# The steps of building a remote network

### Aurora
A high-end, self-managing filing system that keeps all the database safe, organized, and quickly accessible, even if one of the database instances has a problem.

Aurora Serverless

Up to 15 Aurora Read Replicas in a single Aurora DB Cluster

Use Perform on Demond backups to store long-term backups for your Aurora database for disaster recovery and audit purposes

Aurora cloning




#### ElastiCache
ElastiCache and RDS Read Replicas do indeed help with scaling reads.

- Redis
	- Sorted sets
- Memcached


Multi-AZ helps when plan a disaster recovery for an entire AZ going down. If plan against an entire AWS Region going down, use backups and replication across AWS Regions.

#### Security
Encrypt an unencrypted RDS DB instance
- Create a snapshot of the unencrypted RDS DB instance, copy the snapshot and tick "Enable encryption", then restore the RDS DB instance from the encrypted snapshot

Oracle does **NOT** support IAM Database Authentication

Can not create encrypted Read Replicas from an unencrypted RDS DB instance.

Your application running on a fleet of EC2 instances managed by an Auto Scaling Group behind an Application Load Balancer. Users have to constantly log back in and you don't want to enable Sticky Sessions on your ALB as fear it will overload some EC2 instances. What should you do?
- Storing Session Data in ElastiCache is a common pattern to ensuring different EC2 instances can retrieve user's state if needed.

Aurora Global Databases allows you to have an Aurora Replica in another AWS Region, with up to 5 secondary regions.

take a snapshot
restore to point in time
what is the difference then?


### DynamoDB
A NoSQL database service for unstructured data, like a magical notebook that instantly stores and retrieves notes no matter how many you have.
### CloudFront(CDN)
A content delivery network that ensures website's content is delivered quickly to viewers, like having express delivery trucks ready to send your data wherever it needs to go.
- **Overview
	- Improve read performance, content is cached at the edge(the outer reaches of the global network).
	- 216 Point of Presence(PoP) globally => Global Edge network
	- DDoS protection(because worldwide), integration with Shield, AWS web app firewall.
- **Origins
	- **S3
		- distribute files and cache them at the edge
		- Enhanced security with Origin Access Control(OAC), replacing OAI(Identity)
		- CloudFront v.s. S3 Cross Region Replications
			- CloudFront great for static content that must be available everywhere
			- S3 Cross Region Replication: Great for dynamic content that needs to be available at low-latency in few regions.
	- **Custom Origin(HTTP)
		- ALB
		- EC2 instance
		- S3 website
		- Any HTTP backend
- **Geo Restrictions
	- Allowlist & Blocklist
- **Price Classes
- **Cache Invalidation
	- In case update the back-end, use this to force an entire or partial cache refresh

### AWS Global Accelerator
- User => Anycast IP => Edge locations => ALB => application

### Storage Extras
- Snow Family
- Architecture: Snowball into Glacier
- FSx
- Storage Gateway
- Transfer family
- DataSync

### All storage options compared


## 4. Keep the remote computer safe (security and identity services)
### IAM: Identity and Access Management
A security system that ensures only the people you've given keys to can enter the house or specific rooms (services) you've designated.
### Security Groups
Like setting up a security system for your home, deciding who can knock on the door (access your instances) and who can't.
- Another layer of abstraction over IP so that don't need to think about IPs when using groups.
- SSH


## 5. Connect Computer to the Internet (networking and content delivery)

### Route 53
A managed DNS service, acting as the address book translating domain names into IP addresses. The core is how to do the routing?

Route 53 is DNS service, GoDaddy is Domain Registrar. We purchase the domain from GoDaddy and use Route 53 to manage DNS records.

#### Routing policies: define a DNS record (key-value pair with attributes)
- **DNS record types**
	-  A
	- AAAA
	- CNAME
	- NS
	- Alias v.s. CNAME
		- CNAME: points a hostname to any other hostnames
		- Alias: points a hostname to an AWS resource. Free of charge. Native health check.
- **Based on**
	- Simple
	- Weighted: redirect part of the traffic based on weight (e.g. percentage). It's a common use case to send part of traffic to a new version of application.
	- Latency: evaluate the latency between users and AWS Regions, and help them get a DNS response that will minimize their latency (e.g. response time)
	- Failover: instance 1 not healthy, route to instance 2
	- Geolocation:
	- Geoproximity: bias
	- IP
	- Multi-value
- **TTL**
	Each DNS record has a TTL (Time To Live) which orders clients for how long to cache these values and not overload the DNS Resolver with DNS requests. The TTL value should be set to strike a balance between how long the value should be cached vs. how many requests should go to the DNS Resolver.
	- High TTL: Less traffic on Route 53; possibly outdated records
	- Low TTL: More traffic on Route 53; records are outdated for less time; easy to change records so users will experience less downtime.

#### Health checks
- Monitor an endpoint
- Calculated Health checks
- Private Hosted Zones

### VPC: Virtual Private Cloud
A private virtual network to control over the network environment, including IP address range, subnets, routing tables, and network gateways.


### ASG: Auto Scaling Group
The Auto Scaling Group can't go over the maximum capacity (you configured) during scale-out events.
When an EC2 instance fails the ALB Health Checks, it is marked unhealthy and will be terminated while the ASG launches a new EC2 instance.
There's no CloudWatch Metric for "requests per minute" for backend-to-database connections. You need to create a CloudWatch Custom Metric, then create a CloudWatch Alarm.


## 6. Application Integration and Messaging
Decoupling applications while enable internal communications: SQS, SNS, Kinesis, Active MQ
- Sync: app to app. Can be overwhelmed in terms of sudden spikes of traffic.
- Async/Event-based: app to queue to app


## 7. Containers and Orchestration
- **ECR (Elastic Container Registry)**: A Docker container registry for storing, managing, and deploying container images, similar to a photo album for your container snapshots.
- **EKS (Elastic Kubernetes Service)**: Offers managed Kubernetes service for orchestrating containerized applications, acting as the event organizer for managing complex container setups.

## 8. Management and Monitoring

### CloudWatch
A monitoring tool that keeps an eye on the cloud resources and applications, like having a smart home system that alerts if anything unusual happens.

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
