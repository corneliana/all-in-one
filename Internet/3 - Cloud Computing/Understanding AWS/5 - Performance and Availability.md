## ELB: Elastic Load Balancing
Distributes incoming application traffic across multiple targets.
The `smart mail sorting center` for directing internet traffic to different servers.
- Only NLB provides **both static DNS name and static IP**. 
- ALB provides a static DNS name but it does NOT provide a static IP. The reason being that AWS wants ELB to be accessible using a static endpoint, even if the underlying infrastructure that AWS manages changes.

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

## ASG: Auto Scaling Group
Automatically adjusts the number of EC2 instances in response to demand.
- Can't go over the maximum capacity (you configured) during scale-out events.
- When an EC2 instance fails the ALB Health Checks, it is marked unhealthy and will be terminated while the ASG launches a new EC2 instance.

## CloudFront: AWS's Content Delivery Network (CDN)
- Improves read performance, content is cached at the edge(the outer reaches of the global network). Improves user experience.
	- When a user requests content, CloudFront checks its cache to see if the content is already stored at an edge location near the user. If the content is not in the cache or if it has expired, CloudFront retrieves the content from the origin server specified for that particular distribution.
- 216 Point of Presence(PoP) globally => Global Edge network
- DDoS protection(because worldwide), integration with Shield, AWS web app firewall.

- Edge Locations
	- Strategically distributed around the world, often located in or near major cities and Internet exchange points.
	- Leverage Amazon's highly redundant and low-latency network infrastructure, allowing for efficient data transfer. 
	- Cache frequently accessed content, further reducing latency for subsequent requests.
	- Lambda@Edge is a feature of CloudFront that run code closer to users, which improves performance and reduces latency.
- Origins
	- The origin servers or locations where CloudFront retrieves content to serve to end users
	- Can be various types of **servers or storage services**, such as S3 buckets, EC2 instances, ELB, or custom HTTP servers.
		- S3 bucket
			- ![[CloudFront-at-a-high-level.png]]
			- distribute files and cache them at the edge
			- **Enhanced security with Origin Access Control(OAC), replacing OAI(Origin Access Identity)**
			- CloudFront v.s. S3 Cross Region Replications
				- **CloudFront great for static content that must be available everywhere**
				- S3 Cross Region Replication: Great for dynamic content that needs to be available at low-latency in few regions.
		- **Custom Origin(HTTP)
			- ALB
			- EC2 instance
			- S3 website
			- Any HTTP backend
	- Cache and distribute content globally across its network of edge locations.
	- Determines where CloudFront fetches content from and how it delivers that content to users efficiently.
- **Geo Restrictions
	- Allowlist & Blocklist

Key features of CloudFront include:
1. **Global Content Delivery**: operates from a network of edge locations around the world, enabling it to deliver content with low latency to users globally.
2. **Caching**: caches copies of content at edge locations, reducing the need to fetch it from origin server for subsequent requests to lower latency and reduce the load on origin server.
3. **Security**: provides various security features, including SSL/TLS encryption, AWS Shield protection against DDoS attacks, and integration with AWS WAF for web application firewall protection.
4. **Customization**: customize content delivery through features like custom SSL certificates, custom domain names, and support for HTTP/2 and HTTP/3 protocols.
5. **Integration**: seamlessly integrates with other AWS services, such as S3, EC2, Elastic Load Balancing, and Lambda. It can also be used in conjunction with AWS Elemental Media Services for video streaming.
6. **Real-time Analytics**: provides detailed metrics and logs that give insights into your content delivery performance, including viewer demographics, traffic patterns, and cache utilization.

CloudFront is commonly used to accelerate the delivery of static and dynamic content, including websites, APIs, streaming media, and large file downloads. It's **particularly useful for applications with a global user base**, as it helps to ensure a consistent and high-quality user experience across different geographical regions.

In an AWS architecture, CloudFront is often used in conjunction with other services like S3, EC2, and API Gateway to build scalable and performant web applications and APIs. It can sit in front of these services to cache and deliver content more efficiently to end users, thereby improving overall application performance and reducing latency.

Amazon CloudFront can be used in front of an Application Load Balancer.

## Global Accelerator
- User => Anycast IP => Edge locations => ALB => application
- A networking service to intelligently **route non-cacheable TCP and UDP traffic to the nearest healthy endpoint** so as to improve the availability and performance of applications. 
- It optimizes the delivery of dynamic content, such as gaming, IoT, and real-time communications applications, by leveraging the AWS global network infrastructure.

Improve global application availability and performance by directing traffic to the optimal endpoint based on AWS's global network infrastructure. This can potentially provide better performance compared to routing traffic solely through CloudFront.
![![all-in-one/Internet/3 - Cloud Computing/Understanding AWS/#^Table2]]
**Global Accelerator focuses on optimizing the delivery of non-cacheable TCP and UDP traffic to applications, while CloudFront accelerates the delivery of web content by caching it at edge locations.**

## Disaster Recovery
- Disaster: any event that has a negative impact on a company’s business continuity or finances is a disaster.
- Disaster recovery (DR) is about preparing for and recovering from a disaster
- What kind of disaster recovery?  
    - On-premise => On-premise: traditional DR, and very expensive
    - On-premise => AWS Cloud: hybrid recovery  
    - AWS Cloud Region A => AWS Cloud Region B
- Two terms:
	- RPO: Recovery Point Objective
	- RTO: Recovery Time Objective
- Disaster Recovery Strategies
	- Backup and Restore: High RPO  
	- Pilot Light: A small version of the app is always running in the cloud. 
		- Useful for the critical core (pilot light). 
		- Very similar to Backup and Restore. 
		- Faster than Backup and Restore as critical systems are already up  
	- Warm Standby: Full system is up and running, but at minimum size
		- Upon disaster, we can scale to production load
	- Hot Site / Multi Site Approach: Very low RTO (minutes or seconds) – very expensive
		- Full Production Scale is running AWS and On Premise
	- All AWS Multi Region

### DMS: Database Migration Service
- Quickly and securely migrate databases to AWS, resilient, self healing
- The source database remains available during the migration
- Supports:  
    - Homogeneous migrations: ex Oracle to Oracle
    - Heterogeneous migrations: ex Microsoft SQL Server to Aurora
- Continuous Data Replication using CDC
- You must create an EC2 instance to perform the replication tasks
- sources => targets, use SCT(Schema Conversion Tool) if not migrating the same DB engine
- Continuous Replication
- Multi-AZ Deployment
	- When Enabled, DMS provisions and maintains a synchronously stand replica in a different AZ
	- Advantages:  
		- Provides Data Redundancy • Eliminates I/O freezes  
		- Minimizes latency spikes
- RDS & Aurora MySQL Migrations
	- RDS MySQL to Aurora MySQL
		- Option 1: DB Snapshots from RDS MySQL restored as MySQL Aurora DB
		- Option 2: Create an Aurora Read Replica from your RDS MySQL, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)
    - External MySQL to Aurora MySQL
	    - Option 1:
		    - Use Percona XtraBackup to create a file backup in Amazon S3
		    - Create an Aurora MySQL DB from Amazon S3
        - Option 2:
		    - Create an Aurora MySQL DB
		    - Use the mysqldump utility to migrate MySQL into Aurora (slower than S3 method)
    - Use DMS if both databases are up and running
- RDS & Aurora PostgreSQL Migrations
	- RDS PostgreSQL to Aurora PostgreSQL
		- Option 1: DB Snapshots from RDS PostgreSQL restored as PostgreSQL Aurora DB
		- Option 2: Create an Aurora Read Replica from your RDS PostgreSQL, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)
    - External PostgreSQL to Aurora PostgreSQL
	    - Create a backup and put it in Amazon S3  
	    - Import it using the aws_s3 Aurora extension
	- Use DMS if both databases are up and running

- On-Premise strategy with AWS
	- Ability to download Amazon Linux 2 AMI as a VM (.iso format)
		- VMWare, KVM,VirtualBox (Oracle VM), Microsoft Hyper-V
	- VM Import / Export
		- Migrate existing applications into EC2
		- Create a DR repository strategy for your on-premisesVMs
		- Can export back the VMs from EC2 to on-premises
    - AWS Application Discovery Service
		- Gather information about your on-premises servers to plan a migration
		- Server utilization and dependency mappings
		- Track with AWS Migration Hub
    - AWS Database Migration Service (DMS)  
	    - replicate On-premise => AWS , AWS => AWS, AWS => On-premise  
	    - Works with various database technologies (Oracle, MySQL, DynamoDB, etc..)
    - AWS Server Migration Service (SMS)
	    - Incremental replication of on-premises live servers to AWS

### AWS Backup
- Fully managed service  
- Centrally manage and automate backups across AWS services
- No need to create custom scripts and manual processes
- Supported services:  
	- Amazon EC2 / Amazon EBS  
	- Amazon S3  
	- Amazon RDS (all DBs engines) / Amazon Aurora / Amazon DynamoDB
	- Amazon DocumentDB / Amazon Neptune  
	- Amazon EFS / Amazon FSx (Lustre & Windows File Server)  
	- AWS Storage Gateway (Volume Gateway)
- Supports cross-region backups
- Supports cross-account backups
- Supports PITR for supported services
- On-Demand and Scheduled backups
- Tag-based backup policies
- You create backup policies known as Backup Plans  
	- Backup frequency (every 12 hours, daily, weekly, monthly, cron expression) • Backup window  
	- Transition to Cold Storage (Never, Days,Weeks, Months,Years)  
	- Retention Period (Always, Days,Weeks, Months,Years)
![[AWS-Backup.png]]
- Backup Vault Lock
	- Enforce a WORM (Write Once Read Many) state for all the backups that you store in your AWS Backup Vault
	- Additional layer of defense to protect your backups against:
		- Inadvertent or malicious delete operations  
		- Updates that shorten or alter retention periods
	- Even the root user cannot delete backups when enabled

### AWS Application Migration Service (MGN)
- AWS Application Discovery Service
	- Plan migration projects by gathering information about on-premises data centers
	- Server utilization data and dependency mapping are important for migrations
	- Agentless Discovery (AWS Agentless Discovery Connector)
		- VM inventory, configuration, and performance history such as CPU, memory, and disk usage
	- Agent-based Discovery (AWS Application Discovery Agent)
		- System configuration, system performance, running processes, and details of the network connections between systems
	- Resulting data can be viewed within AWS Migration Hub
- AWS Application Migration Service (MGN)
	- The “AWS evolution” of CloudEndure Migration, replacing AWS Server Migration Service (SMS)
	- Lift-and-shift (rehost) solution which simplify migrating applications to AWS
	- Converts your physical, virtual, and cloud-based servers to run natively on AWS
	- Supports wide range of platforms, Operating Systems, and databases
	- Minimal downtime, reduced costs
	
### VMware Cloud on AWS
- Some customers use VMware Cloud to manage their **on-premises Data Center**  
- They want to **extend the Data Center capacity to AWS, but keep using the VMware Cloud software**
- ...EnterVMware Cloud on AWS
- Use cases
	- Migrate your VMware vSphere-based workloads to AWS
	- Run production workloads across VMware vSphere-based private, public, and hybrid cloud environments
	- Have a disaster recover strategy
![[VMware-cloud-on-aws.png]]

## Transfer large amount of data into AWS
- Example: transfer 200 TB of data in the cloud. We have a 100 Mbps internet connection.
- Over the internet / Site-to-Site VPN:  
    • Immediate to setup  
    • Will take 200(TB)*1000(GB)*1000(MB)*8(Mb)/100 Mbps = 16,000,000s = 185d
- Over direct connect 1Gbps:  
    • Long for the one-time setup (over a month)  
    • Will take 200(TB)*1000(GB)*8(Gb)/1 Gbps = 1,600,000s = 18.5d
- Over Snowball:
    • Will take 2 to 3 snowballs in parallel  
    • Takes about 1 week for the end-to-end transfer • Can be combined with DMS
- For on-going replication / transfers: Site-to-SiteVPN or DX with DMS or DataSync