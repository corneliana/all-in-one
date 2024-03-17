AWS: Amazon Web Services, a cloud computing platform. Cloud computing is a new layer of abstraction over physical computing resources, which transforms them into scalable, on-demand services that can be used without managing the underlying hardware directly.

The platform is used in a specific region => **Availability Zones (AZ)**
- global services: IAM, Route 53(DNS Service), CloudFront(Content Delivery Network), WAF(Web Application Firewall)
- regional services: EC2(IaaS), Elastic Beanstalk(PaaS), Lambda(FaaS), Rekognition(SaaS)

In a sense, the whole idea of AWS is like assembling a remote computer, along with its network, piece by piece according to the needs of users.

## 1. Choose the right type of computers to compute and host
### EC2: Elastic Compute Cloud = Infrastructure as a Service
An Amazon EC2 instance is like a virtual computer running in the cloud, similar to CPU and RAM of a computer. Choose its types and sizes based on computational power and memory requirements to determine the performance capabilities.
- **Purchasing Options:** 
	- On-Demand: pay the full price for what you use at any time
	- Reserved: 1-3 year for a discount
	- Saving plans: pay a certain amount to a certain type and region on long-term usage
	- Spot: the highest bidder pays, i.e. your max price > current spot price. Most cost-efficient
	- Dedicated Hosts / Instance: EC2 instance capacity fully dedicated to your use
	- Capacity Reservations: Regional reserved instance + saving plans (pay full price for a period even you don't use it)
	- spot fleets = set of Spot instances + (optional) On-demand instances
	- 
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

### Fargate
A serverless compute engine for containers, allowing you to run containers without managing servers or clusters.
### EKS: Elastic Kubernetes Service
A managed container orchestration service for running Kubernetes on AWS without having to set up or maintain your own Kubernetes control plane.
### Lambda
For tasks that don't need a dedicated computer running all the time. It's like having robots wake up, perform a task quickly, and go back to sleep without worrying about the electricity bill.

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

### S3: Simple Storage Service
Think of S3 as an infinitely large external hard drive or cloud storage service to store and retrieve any amount of data, at any time, from anywhere on the web. It's great for data backup, archiving, and serving web content.

### Aurora
A high-end, self-managing filing system that keeps all the database safe, organized, and quickly accessible, even if one of the database instances has a problem.
### RDS: Relational Database Service
A managed database service, like having a specialized filing cabinet for the structured data that takes care of organizing, retrieving, and storing efficiently.
### DynamoDB
A NoSQL database service for unstructured data, like a magical notebook that instantly stores and retrieves notes no matter how many you have.
### CloudFront
A content delivery network that ensures website's content is delivered quickly to viewers, like having express delivery trucks ready to send your data wherever it needs to go.


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

### VPC: Virtual Private Cloud
A private virtual network to control over the network environment, including IP address range, subnets, routing tables, and network gateways.
### ELB: Elastic Load Balancing
The "smart mail sorting center" for directing internet traffic to different servers.
### ASG: Auto Scaling Group
### Route 53
A managed DNS service, acting as the address book translating domain names into IP addresses.

## 6. Application Integration and Messaging
- **SNS (Simple Notification Service)**: The "town crier" for broadcasting messages or alerts.
- **SQS (Simple Queue Service)**: The "post office line" for managing messages or tasks in a queue, ensuring they're processed in order.
- **Kinesis**: A platform for collecting, processing, and analyzing real-time, streaming data, akin to monitoring and managing the flow of information.
- **Active MQ**: A message broker service for communicating between different parts of your application, similar to a bus system for messages.

## 7. Containers and Orchestration
- **ECR (Elastic Container Registry)**: A Docker container registry for storing, managing, and deploying container images, similar to a photo album for your container snapshots.
- **EKS (Elastic Kubernetes Service)**: Offers managed Kubernetes service for orchestrating containerized applications, acting as the event organizer for managing complex container setups.

## 8. Management and Monitoring

### CloudWatch
A monitoring tool that keeps an eye on the cloud resources and applications, like having a smart home system that alerts if anything unusual happens.

## 9. Building and Deploying Application (Developer Tools)

### CodeDeploy, CodeBuild, CodePipeline
These services automate code deployment, building, and integration process, akin to having a team of robots that continuously improve and fix your house without you lifting a finger.


### Aurora

### Elastic Cache

### SNS, SQS

### Kinesis, Active MQ

### Fargate, ECR, EKS


## Quiz

You are running a high-performance database that requires an IOPS of 310,000 for its underlying storage. What do you recommend?

Use an EC2 instance store

You can run a database on an EC2 instance that uses an Instance Store, but you'll have a problem that the data will be lost if the EC2 instance is stopped (it can be restarted without problems). One solution is to set up a replication mechanism on another EC2 instance with an Instance Store to have a standby copy. Another solution is to set up backup mechanisms for your data. It's all up to you how you want to set up your architecture to validate your requirements. In this use case, it's around IOPS, so we have to choose an EC2 Instance Store.







