# [[OSI (Open Systems Interconnection) model]]

1. **Physical Layer (OSI Layer 1)**:
    - AWS Direct Connect: Establishes a dedicated network connection between AWS and an on-premises data center, bypassing the public internet.
2. **Data Link Layer (OSI Layer 2)**:
    - N/A
3. **Network Layer (OSI Layer 3)**:
    - Amazon VPC (Virtual Private Cloud): Provides a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network.
	- Amazon Route 53: DNS web service for routing end users to Internet applications.
4. **Transport Layer (OSI Layer 4)**:
    - Global Accelerator: Improves the availability and performance of your applications with local and global traffic management.
	- Network Load Balancer: Distributes incoming application traffic across multiple targets, such as EC2 instances.
5. **Session Layer (OSI Layer 5)**:
    - N/A
6. **Presentation Layer (OSI Layer 6)**:
    - N/A
7. **Application Layer (OSI Layer 7)**:
    - Compute Services:
	    - EC2 (Elastic Compute Cloud): Provides resizable compute capacity in the cloud.
	    - Lambda: Runs code without provisioning or managing servers.
	- Storage Services:
	    - S3 (Simple Storage Service): Object storage service designed to store and retrieve any amount of data.
	    - EBS (Elastic Block Store): Provides block-level storage volumes for use with EC2 instances.
	- Database Services:
	    - RDS (Relational Database Service): Managed relational database service supporting various database engines like MySQL, PostgreSQL, etc.
	    - DynamoDB: Fully managed NoSQL database service.
	- Networking Services:
	    - API Gateway: Fully managed service for creating, publishing, maintaining, monitoring, and securing APIs at any scale.
	    - CloudFront: Content delivery network (CDN) service for fast and secure delivery of data, videos, applications, and APIs.
	    - Elastic Load Balancing (ELB): Automatically distributes incoming application traffic across multiple targets.
	    - Route 53: Scalable DNS web service for routing end users to Internet applications.

AWS: Amazon Web Services, a cloud computing platform. Cloud computing is a new layer of abstraction over physical computing resources, which transforms them into scalable, on-demand services that can be used without managing the underlying hardware directly.

The platform is used in a specific region => **Availability Zones (AZ)**
- global services: IAM, Route 53(DNS Service), CloudFront(Content Delivery Network), WAF(Web Application Firewall)
- regional services: EC2(IaaS), Elastic Beanstalk(PaaS), Lambda(FaaS), Rekognition(SaaS)

In a sense, the whole idea of AWS is like assembling a remote computer, along with its network, piece by piece according to the needs of users.


# The steps of building a remote network
## 1. Choose the right type of computers to compute and host
### EC2: Elastic Compute Cloud = Infrastructure as a Service
An Amazon EC2 instance is like a virtual computer running in the cloud, similar to CPU and RAM of a computer. Choose types and sizes based on computational power and memory requirements to determine the performance capabilities.

- **Purchasing Options:** 
	- On-Demand: pay the full price for what you use at any time
	- Reserved: 1-3 year for a discount
	- Saving plans: pay a certain amount to a certain type and region on long-term usage
	- Spot: the highest bidder pays, i.e. your max price > current spot price. Most cost-efficient
	- Dedicated Hosts / Instance: EC2 instance capacity fully dedicated to your use
	- Capacity Reservations: Regional reserved instance + saving plans (pay full price for a period even you don't use it)
	- spot fleets = set of Spot instances + (optional) On-demand instances

- **Address => IP: Public / Private / Elastic** 
	- Private v.s. Public IP(IPv4): whether the machine can be identified on the public network
	- Elastic IPs: a fixed public IPv4 IP for the instance. 
		- Efficient backup by remapping the address to another instance if failure occurs.
		- Try not use it as it often reflects poor architecture. Instead, use a random public IP and register a DNS name to it. Or, use a Load Balancer and don't use a public IP. In other words, a flexible, dynamic addressing system to the cloud-based system with scalability, resilience, and efficient traffic management.
	 - Elastic Network Interfaces (ENI): a virtual network card that gives EC2 instance access to the network.
	
- **Organization / Orchestration => Placement Groups**: put the instances in
	- Cluster: in the same rack of the same AZ. => big data job, low latency and high network throughput.
	- Spread: across underlying hardware (max 7 instances per group per AZ)  => maximize high availability as each instance is isolated from failure from each other.
	- Partition: across many different partitions (which rely on different sets of racks) within an AZ. Scales to 100s of EC2 instances per group (Hadoop, Cassandra, Kafka)
	
- **Hibernate:** preserve in-memory(RAM) state so as to accelerate booting. Under the hood, the RAM state is written to a file in the root EBS volume which must be encrypted. Available for On-demand, Reserved and Spot instances, and can't be hibernated more than 60 days.

- **AMI(Amazon Machine Image):** a customization of an EC2 instance, which is built for a specific region and can be copied across regions.
	- Build an AMI - this will also create EBS snapshots.
	Golden AMI is an image that contains all your software installed and configured so that future EC2 instances can boot up quickly from that AMI.


### Containers on AWS: ECS, Fargate, ECR & EKS
- **Docker
	- Docker is software development platform to deploy apps. Apps are packaged in containers that can be run on any OS. Docker images are stored in Docker repos.
	- v.s. Virtual Machines
		- Resources are shared with the host. Many containers on one server.
	- Dockerfile => Docker Repository => use
- **Docker Containers Management on AWS
	- **ECS Elastic Container Service: auto deployment, scaling and management of containerized applications
		- Mechanism
			- Cluster management: a grouping of container instances (EC2 instances or Fargate tasks) that are managed together as a single unit
			- Resource allocation
			- Task Scheduling
			- Scaling and availability: AWS Application Auto Scaling (task level, not instance level)
		- launch type
			- EC2 launch type: Launch Docker containers on AWS = **Launch ECS Tasks on ECS clusters**. Each EC2 instance must run the ECS agent to register in the ECS cluster.
				- Scaling: ASG or ECS Cluster Capacity Probider
			- Fargate launch type: run containers on AWS without managing any servers.
		- IAM roles 
			- EC2 instance profile
			- ECS task role: IAM Role used by the ECS task itself. Use when container wants to call other AWS services like S3, SQS, etc.
		- LB integrations: ALB, NLB
		- Data Volumes(EFS): Fargate + EFS = Serverless
			- EFS volume can be shared between different EC2 instances and different ECS Tasks. It can be used as a persistent multi-AZ shared storage for containers.
	- Fargate: A serverless compute engine for containers, running containers without managing servers or clusters.
	- ECR Elastic Container Registry: Store and manage Docker images on AWS
		- A fully managed container registry that makes it easy to store, manage, share, and deploy container images. ECR is fully integrated with ECS, allowing easy retrieval of container images from ECR while managing and running containers using ECS.
	- **EKS Elastic Kubernetes Service**: An alternative to ECS and open-source. A managed container orchestration service for running Kubernetes on AWS. EKS nodes => EKS pods = ECS tasks.
		- Node types
			- Managed Node Groups
			- Self-Managed Nodes
			- Fargate: no nodes
		- Data volumes
- **AWS App Runner

## 2. Set up the hard drive (storage solutions)
Note: Storage is where data is kept long-term, and memory is where data is kept short-term for quick access by the computer's processor.

### EBS: Elastic Block Store
Like a physical computer needs a hard drive to **store its operating system, applications, and data**, an EC2 instance requires a virtual hard drive. This is where EBS volumes come into play.
EBS is a network drive, so uses the network to communicate the instance.
**Volume v.s. Snapshot**
- Volume is a durable, block-level **storage device** that is attached to a single EC2 instance to ensure persistent, flexible, and high-performance storage.
- To move a volume across AZs, snapshot it first. Snapshot is a point-in-time copy of a Volume, which are stored in S3 and provide a historical record of a volume's state, ensuring a robust backup and disaster recovery.
	- Archive: Move a snapshot to an "archive tier" that is 75% cheaper.
	- Recycle bin: Set up rules to retain deleted snapshots so they can be recovered.
	- Fast snapshot restore(FSR): Force full initialization to have no latency on the first use.
- **EBS Volume Types:**
	- **General Purpose SSD:** cost-effective usage, system boot volumes, virtual backups, development and test environments. gp2/gp3
	- **Provisioned IOPS(PIOPS) SSD:** sustained IOPS performance(6000), database workloads. Supports EBS Multi-attach. io1/io2, io3 Block Express.
	- **Hard Disk Drives(HDD):** Cannot be a boot volume.
		- Throughput Optimized HDD(stl): big data, data warehouse, log processing.
		- Cold HDD(scl): for data that is frequently accessed, lowest cost.
- **EBS Multi-attach:** a EBS volume attached to multiple EC2 instances in the same AZ. Each instance has full read/write permissions. Up to 16 EC2 instances at a time.
	- Achieve higher application availability in clustered Linux applications.
	- Application must manage concurrent write operations.
- **EBS encryption:** data inside the volume/in flight between the instance and volume/snapshots/volumes created from the snapshot are encrypted.

EBS volumes are created in a specific AZ and can only be attached to one EC2 instance at a time.

### EC2 Instance Store 
higher-performance hardware disk than EBS
- Better I/O performance
- Lose storage if stopped(ephemeral)
- Good for buffer/cache/scratch data/temporary content
- Risk of data loss if hardware fails
- Backups and replication are your responsibility

### Elastic Cache
A special memory box for storing copies of the most frequently used items (data caching) so we can get them quickly without searching through the entire house.

## 3. Organize and Access data (db and content delivery)
### EFS: Elastic File System
Managed NFS(network file system) that can be mounted on many EC2 instances in multi-AZ. Highly available, scalable, elastic, it allows multiple EC2 instances to access the file system at the same time, much like sharing files across different computers on the same network.
- content management, web serving, data sharing, wordpress.

EFS is a network file system (NFS) that allows mounting the same file system to 100s of EC2 instances. Storing software updates on an EFS allows each EC2 instance to access them.

### S3: Simple Storage Service
Think of S3 as an infinitely large external hard drive or cloud storage service to store and retrieve any amount of data, at any time, from anywhere on the web. It's great for data backup, archiving, and serving web content.
#### Versioning the buckets
- Enabled at the bucket level
#### Security
- **pre-signed URL v.s. public URL**
	- The former contains a signature that carries the user's credentials in the URL, which is inherited from the permissions of the user that generated the URL for GET/PUT.
	- S3 Pre-Signed URLs are temporary URLs that you generate to grant time-limited access to some actions in your S3 bucket.
- **Policies
	- Explicit DENY in an IAM Policy will take precedence over an S3 bucket policy.
	- User-based
		- IAM Policies
	- Resource-based
		- Bucket Polices
			- JSON based policies: Resource, Effect, Actions, Principal
			- Use it to grant access to buckets/accounts, encrypt objects at upload
			- Use cases
	- Object ACL(Access Control List)
	- Bucket ACL
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
- **CORS
	- Cross-Origin Resource Sharing: defines a way for client web applications that are loaded in one domain to interact with resources in a different domain
- **MFA Delete
	- Extra protection against permanent deletion, which forces users to use MFA codes before deleting S3 objects. It's an extra level of security to prevent accidental deletions.
	- `aws s3api put-bucket-versioning --bucket demo-stephane-mfa-delete-2020 --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "arn:aws:iam::XXXXXXXXXX:mfa/root-account-mfa-device XXXXXX --profile root-mfa-delete-demo(this is the profile)`
- **Glacier Valut Lock & S3 Object Lock
	Adopt a WORM model (Write Once Read Many)
	- **Glacier
		- Create a Vault Lock Policy
		- Lock the Policy for future edits
	- **Object
		- Block an object version deletion
		- Retention mode
			- Compliance
			- Governance: more lenient than compliance
		- Legal Hold: protect the object indefinitely, independent from retention period
- **Access Logs
	- Log all access to S3 buckets
	- Amazon Athena can then be used to run serverless analytics on top of the log files.
- **Access Points
	- To avoid unmanageable policies and manage security at scale.
	- Specific points to access corresponding resources.
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
	
#### Performance
- **Store
	- Baseline
	- Multi-Part upload
	- Transfer acceleration
- **Read
	- S3 Byte-Range Fetches
	- S3 Select & Glacier Select
- **Batch Operations

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
### RDS: Relational Database Service
A managed database service, like having a specialized filing cabinet for the structured data that takes care of organizing, retrieving, and storing efficiently.
RDS supports MySQL, PostgreSQL, MariaDB, Oracle, MS SQL Server, and Amazon Aurora.

Backup for better performance and avoid disaster.

RDS Proxy vs multi-AZ in terms of re-connecting to DB.

#### Read replica
RDS database can have up to 15 Read Replicas.
Read Replicas have Asynchronous Replication, therefore it's likely users will only read Eventual Consistency.

#### RDS in multi-AZ
Multi-AZ keeps the same connection string regardless of which database is up.
Multi-AZ won't help when a disaster happens at the AWS region level. Multi-AZ helps when a disaster happens at the AZ level.

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

### SQS: Simple Queue Service => queue model
A fully managed message queuing service. Producer sends messages to SQS queue, and Consumers poll messages from the queue.
- **Mechanism
	- **Produce and send**: The message will be sent again if not processed in time.
	- **Consume, process and delete**: consumers can be run on EC2/servers/lambda... Consumers can be scaled to handle higher throughput.
		- ASG: to handle big load
			- requests => ASG(enqueue) => SQS => ASG(dequeue) => DB
		- WaitTimeSeconds to start Long Polling: consumers wait for messages to arrive if there is none in the queue.
		- Message Visibility Timeout: Message that being polled will be invisible to other consumers for a period of time. If not deleted, the same message will back to the queue after being timeout is elapsed.
		- ChangeMessageVisibility API to get more time for processing.
	- **Ordering of messages**: FIFO
		- The order in which messages are sent and received are strictly preserved and a message is delivered once and remains available until a consumer process and deletes it. 
		- Duplicated messages are not introduced into the queue.
	
- **Use cases
	- Decouple applications
	- A buffer to DB writes

- **Security
	- Encryption
		- In-flight: HTTPS
		- At-rest: KMS
		- Client-side
	- Access Controls: IAM
	- Access Policies: similar to S3

- **Dead Letter Queue

### SNS: Simple Notification Service => pub/sub model
 The "town crier" for broadcasting messages or alerts, i.e. send messages to many receivers. SNS is a fully managed pub/sub (publish/subscribe) messaging service, enabling publish messages to topics, which are logical channels or categories.
- **Mechanism
	- Event producer => SNS topics <= receivers listen to SNS topic notifications
		- Producers: CloudWatch, Budgets, Lambda, ASG, S3, DynamoDB, CloudFormation(state changes), DMS(New Replic), RDS events
			- Topic publish
			- Direct Publish
- **Security
	- Encryption
		- In-flight: HTTPS
		- At-rest: KMS
		- Client-side
	- Access Controls: IAM
	- Access Policies: similar to S3
- **Ordering
	- FIFO
- **Message filtering using JSON policy**
	- SNS => filtering policy => different queues for corresponding messages
### SNS + SQS: Fan out
Push once in SNS, receive in all SQS queues that are subscribers. This is a common pattern where only one message is sent to the SNS topic and then "fan-out" to multiple SQS queues. This approach has the following features: it's fully decoupled, no data loss, and you have the ability to add more SQS queues (more applications) over time.
- S3
	- S3 Events => SNS topic => SQS queues
	- SNS => Kinesis Data Firehose => S3
- SNS FIFO + SQS FIFO: Fan out => fan out + ordering + deduplication

SNS facilitates the broadcasting of messages to multiple subscribers through topics, SQS enables the reliable and scalable queuing of messages between different components of a system, helping to decouple components and manage async communication. These two are often used together in AWS architectures to build scalable and resilient distributed applications.
### Kinesis => real-time streaming model
A platform for collecting, processing, and analyzing real-time, streaming data, akin to monitoring and managing the flow of information.
- **Data streams
	- Performance
		- The capacity limits of a Kinesis data stream are defined by the number of shards within the data stream. The limits can be exceeded by either data throughput or the number of reading data calls. Each shard allows for 1 MB/s incoming data and 2 MB/s outgoing data. You should increase the number of shards within the data stream to provide enough capacity.
	- Producers: produce records(partition key + data blob) into stream
		- `aws kinesis put-record --stream-name Demostream --partition-key user1 --data "user signup" --cli-binary-format raw-in-base64-out`
		- `aws kinesis describe-stream --stream-name Demostream
		- `aws kinesis get-shard-iterator --stream-name Demostream --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON
		- `aws kinesis get-records --shard-iterator "AAAAAAAAAAG5U/7UVKD+rFGfmq6suBhb2WCaqL+GN9sgXkxhE7vPheX1rpI1Yml5EGyV/JNu5+wlXUGdeKVOo8hUOUGvOE6qw6aRgbdG/yhYX1ow8SOdExxDlIr1t7tu0C3FsMjKteNUR8GBv8LzfI3wCDCrZa9OZ+4GLxlCgnIYgSKzHGn6NAMvDDOsIcdHP8oqYPUn98x1Q5Hh+stuBW+axU3tZeYHl+g8fdcq5nCg9r8PLhRbDA=="`
	- Shards: define capacity
	- Consumers: Receive and consume the records. The consumers can be Apps(KCL, SDK), lambda, Data firehose, Data Analytics...
		- Write you own
	- Capacity Modes
		- Provisioned mode
		- On-demand mode
	- Security: the same as SQS and SNS + VPC, Monitor API calls using CloudTail
- **Data firehose
	- Producers => Firehose => Batch writes to Destinations
		- Destinations
			- AWS: Redshift / S3 / OpenSearch
			- 3rd party partner: Splunk / MongoDB / DataDog / NewRelic
			- Custom: send to any HTTP endpoint
- **Data streams v.s. Data firehose

#### Ordering data 
- into Kinesis
	- Using partition key. The same key will always go to the same shard. Kinesis Data Stream uses the partition key associated with each data record to determine which shard a given data record belongs to. Using the identity of each user as the partition key ensures the data for each user is ordered hence sent to the same shard.
- into SQS FIFO
	- No GroupID, messages are consumed in the order sent, with only one consumer.
	- With GroupID, the number of consumers can be scaled.

#### SQS v.s. SNS v.s. Kinesis


### MQ
A message broker service for communicating between different parts of your application. RabbitMQ, ActiveMQ
Amazon MQ supports industry-standard APIs such as JMS and NMS, and protocols for messaging, including AMQP, STOMP, MQTT, and WebSocket.

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
