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


AWS: Amazon Web Services, a cloud computing platform. Cloud computing is a new layer of abstraction over physical computing resources, which transforms them into scalable, on-demand services that can be used without managing the underlying hardware directly.

The platform is used in a specific region => **Availability Zones (AZ)**
- global services: IAM, Route 53(DNS Service), CloudFront(Content Delivery Network), WAF(Web Application Firewall)
- regional services: EC2(IaaS), Elastic Beanstalk(PaaS), Lambda(FaaS), Rekognition(SaaS)

In a sense, the whole idea of AWS is like assembling a remote computer, along with its network, piece by piece according to the needs of users.


# The steps of building a remote network

### S3: Simple Storage Service

- **Encryption
	- **Default Encryption v.s. Bucket polices
		- Default Encryption is auto applied and bucket polices are added.
		- Bucket policies are evaluated before Default Encryption
	- **In objects
		- **Server-Side Encryption(SSE)
			- Using keys handled, managed and owned by AWS, using AES-256
			- Enabled by default for new buckets / objects
		- **SSE-KMS
			- key is from KMS
			- May be impacted by the KMS limits
			- The encryption keys are managed by AWS but you have full control over the rotation policy of the encryption key.
		- **SSE-C
			- keys managed by the customer outside of AWS
			- HTTPS is mandatory
		- **[DSSE-KMS(Dual-Layer SSE with Keys Stored in Key Management Service)](https://aws.amazon.com/blogs/aws/new-amazon-s3-dual-layer-server-side-encryption-with-keys-stored-in-aws-key-management-service-dsse-kms/)
		- **Client-Side Encryption(CSE)
	- **In transit/flight: SSL/TLS
		- HTTPS
		- Force encryption => use the bucket policy of aws:SecureTransport

#### Storage
- **Services provided: Static website hosting**
	- S3 can host static websites and have them accessible on the Internet. If 403, makes sure the bucket policy allows public reads.
- **Storage classes: Durability and Availability
	- **Types and Payment
		- Standard
		- Standard-Infrequent: but requires rapid access when needed.
		- One Zone-Infrequent: High durability in a single AZ; data lost when AZ is destroyed.
		- Glacier
			- Instant Retrieval
			- Flexible Retrieval: Expedited, Standard, Bulk => wait to retrieve data
			- Deep Archive
		- Intelligent-Tiering
			- Frequent
			- InFrequent
			- Archive Instant Access
			- Archive Access
			- Deep Archive
	- **Moving between storage classes: manually or use class lifecycle configuration
		- Lifecycle rules
		- Use S3 Analytics to analyze the optimal number of days to move objects between different storage tiers
	- **Replications: CRR(Cross-Region) & SRR(Same-Region)
		- Enable a one-time Batch Operations job from this replication configuration to replicate objects that already exist in the bucket and to synchronize the source and destination buckets.

#### Payment
- In general, bucket owner pays for all the storage and data transfer costs associated with the bucket.
- With requester pays bucket, the requester pays the cost of the request and the data downloaded from the bucket. Helpful when share large datasets with other accounts.
#### Event notifications
- Event: CRUD operations on the objects in S3
	- Events => S3 => SNS / SQS / Lambda
	- Events => EventBridge (Advanced filtering options with JSON rules) => over 18 services as destinations
	- IAM Permissions: Resource access policy
	
#### Storage Lens
- Metrics
	- Summary Metrics: general insights about storage, objects
	- Cost-Optimization Metrics
	- Data-Protection Metrics
	- Access-management Metrics
	- Event Metrics
	- Performance Metrics: transfer acceleration
	- Activity: how the storage is requested
	- Detailed Status Code: HTTP status code
- Payment
	- Free Metrics: 28
	- Advanced Metrics

#### S3 lambda
- Use lambda to change the object before it is being retrieved.
- E.g. redact PII data, convert data format, resize and watermark images, etc.
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
### KMS: Key Management Service
A highly secure vault to keep all the encryption keys for resources. Only those with permission can access these keys to unlock or secure the valuable information.

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
### ELB: Elastic Load Balancing
The `smart mail sorting center` for directing internet traffic to different servers.
Only NLB(Network) provides **both static DNS name and static IP**. ALB(Application) provides a static DNS name but it does NOT provide a static IP. The reason being that AWS wants ELB to be accessible using a static endpoint, even if the underlying infrastructure that AWS manages changes.

#### Stickiness / Session affinity
ELB Sticky Session feature ensures traffic for the same client is always redirected to the same target (e.g., EC2 instance). This helps that the client does not lose his session data.

The following cookie names are reserved by the ELB (AWSALB, AWSALBAPP, AWSALBTG).

When using an ALB to distribute traffic to EC2 instances, the IP address that receives requests from will be the ALB's private IP addresses. To get the client's IP address, ALB adds an additional header called "X-Forwarded-For" contains the client's IP address.
Application Load Balancers support HTTP, HTTPS and WebSocket.
ALBs can route traffic to different Target Groups based on URL Path, Hostname, HTTP Headers, and Query Strings.

NLB provides the highest performance and lowest latency if the app needs it.
NLB has one static IP address per AZ and you can attach an Elastic IP address to it. ALB and Classic Load Balancers have a static DNS name.
NLB supports HTTP health checks as well as TCP and HTTPS

Server Name Indication (SNI) allows you to expose multiple HTTPS applications each with its own SSL certificate on the same listener. Read more here: https://aws.amazon.com/blogs/aws/new-application-load-balancer-sni/

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
