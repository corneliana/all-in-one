**Keys:**
- Storage: how much data and for how long? Will it grow? Average object size? How are they accessed? Data durability? Source of truth for the data?
- Performance: I/O and throughput needs? Will it change, does it need to scale or fluctuate during the day?
- Availability: Latency requirements? Concurrent users?
- Security
- Lifecycle
- Event notifications/log

RDS

- **Relational Databases v.s. Non-Relational Databases**
	- Relational databases organize data into **structured tables** with **predefined schemas**, and they use SQL (Structured Query Language) for querying and manipulating data. They are suitable for applications with complex data relationships and transactions, such as e-commerce platforms, financial systems, and customer relationship management (CRM) applications.
	    - RDS (Relational Database Service), which supports various engines like MySQL, PostgreSQL, Oracle, SQL Server, and MariaDB
	    - Amazon Aurora, which is a high-performance, fully managed relational database engine compatible with MySQL and PostgreSQL.
    
	- Non-relational databases, also known as NoSQL databases, store and retrieve data in formats other than traditional row-column tables used in relational databases. They are designed for scalability, flexibility, and performance, and they are suitable for applications with large volumes of unstructured or semi-structured data, such as social media platforms, IoT (Internet of Things) systems, and real-time analytics.
	    - DynamoDB, a fully managed NoSQL database service offering low-latency, high-throughput storage for applications that require single-digit millisecond latency at any scale
	    - Amazon DocumentDB (with MongoDB compatibility), a fully managed document database service compatible with MongoDB workloads.

## EFS: Elastic File System
- **What is EFS? v.s EBS?**
	- Managed NFS (network file system) that can be mounted **on many EC2 in multi-AZ**
	- allow multiple EC2 instances to access the file system at the same time
	- use cases: content management, web serving, data sharing, wordpress.

- **Storage Classes**
	- Storage Tiers (lifecycle management feature – move file after N days)
		- Standard: for frequently accessed files
		- Infrequent access (EFS-IA): cost to retrieve files, lower price to store. **Enable EFS-IA with a Lifecycle Policy**
	- Availability and durability
		- Standard: Multi-AZ, great for prod
		- One Zone: One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA)

NFS: Network File System, a distributed file system protocol that allows a user on a client computer to access files over a network as if they were stored locally on the client's own hard drive.
vs Google Drive
- serve similar purposes in providing file storage and access, but use different technologies and architectures. Google Drive is a cloud-based storage service with its own protocols, while NFS is a network file system protocol designed for sharing files over a local or wide area network. Google Drive is a cloud-based service designed for personal and collaborative file storage and sharing, while NFS is a network file system protocol primarily used for centralized file storage and sharing within traditional IT environments.

## S3: Simple Storage Service
An infinitely large **external hard drive** or cloud storage service to store and retrieve any amount of data, at any time, from anywhere on the web.
S3 is commonly used for storing large amounts of data in a scalable and cost-effective manner, great for data backup, archiving, and serving web content. 

### Keys
- **Buckets** (dictionaries) to store objects (files) **at regional level**.
- **Versioning** files at bucket level. Must enable versioning in source and destination buckets.
- **Replication** enables automatic, asynchronous copying of objects across buckets.
	- Cross-region replication
- **Lifecycle**: move between storage classes or using S3 Lifecycle configurations
	- ![[s3-storage-classes.png]]
- **Performance:**
	- **Store
		- Baseline
		- Multi-Part upload: big files divided into parts
		- **Transfer acceleration** 
			- transfer files to AWS Edge Location(part of CloudFront) that are are optimized for fast content delivery by leveraging AWS's global network infrastructure, routing data through Edge Locations closest to the uploader.
			- then forward file to the S3 bucket in the target region within the AWS network backbone.
	- **Read
		- S3 Byte-Range Fetches
		- S3 Select & Glacier Select
	- **Batch Operations
- **Security**
	- Object Encryption
		- Server-Side Encryption
			- S3-Managed Keys - Enabled by default. AES-256
			- ![[SSE-S3.png]]
			- KMS keys stored in KMS (SSE-KMS): use KMS to manage encryption keys
			- ![[S3-KMS.png]]
				- KMS limits
					- Upload calls the GenerateDataKey KMS API
					- Download calls the Decrypt KMS API
					- Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region)
					- You can request a quota increase using the Service Quotas Console
			- Customer-provided keys (SSE-C): manage your own encryption keys outside of AWS. HTTPS must be used.
			- ![[S3-SSE-Customer-provided-key.png]]
		- Client-Side Encryption
			- Use client libraries such as Amazon S3 Client-Side Encryption Library
			- Clients must encrypt data themselves before sending to Amazon S3  
			- Clients must decrypt data themselves when retrieving from Amazon S3
			- Customer fully manages the keys and encryption cycle
			- ![[S3-CSE.png]]
		- Encryption in transit(SSL/TLS)
			- S3 exposes two endpoints:
				- HTTP Endpoint – non encrypted  
				- HTTPS Endpoint – encryption in flight, recommended and mandatory for SSE-C 
			- Most clients would use the HTTPS endpoint by default
			- ![[S3-Force-encryption.png]]
		- Note
			- SSE-S3 encryption is automatically applied to new objects stored in S3 bucket
			- Optionally, you can “force encryption” using a bucket policy and refuse any API call to PUT an S3 object without encryption headers (SSE-KMS or SSE-C)
			- Bucket Policies are evaluated before “Default Encryption”
	- **pre-signed URL v.s. public URL**
		- pre-signed URL contains a signature that carries the user's credentials, inherited from the permissions of the user that generated the URL. They are temporary URLs that you can grant time-limited access to some actions.
	- **Policies
		- Explicit `DENY` in an IAM Policy will take precedence over an S3 bucket policy.
		- **User-based:** IAM Policies, API calls should be allowed for a specific user from IAM.
		- **Resource-based**: grant access to buckets/accounts, encrypt objects at upload
			- Bucket Policies
				- JSON based policies: Resource, Effect, Actions, Principal
				- Use cases
			- Object ACL(Access Control List): finer grain
			- Bucket ACL: less common
		- Note: an IAM principal can access an S3 object if 
			- The user IAM permissions ALLOW it OR the resource policy ALLOWS it
			- AND there’s no explicit DENY

	- Using S3 as a secure transfer point: a location or service within a system where data can be transferred securely between different entities. It often involves a service or mechanism that ensures the confidentiality, integrity, and availability of data during its transfer. This can include encryption of data in transit, authentication mechanisms to verify the identity of parties involved in the transfer, and protection against unauthorized access or interception. 
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

S3 File Gateway: a type of AWS Storage Gateway that extends on-premises file storage to the cloud, i.e. access and store files in S3 using standard file protocols such as NFS (Network File System) and SMB (Server Message Block).

With S3 File Gateway, the most recently accessed files can be cached locally for low-latency access, ensuring that users can quickly access frequently accessed files.


## Aurora
MySQL and PostgreSQL-compatible relational database that offers the performance and availability of commercial-grade databases at little cost.
It is fully managed by AWS, meaning AWS handles administrative tasks such as provisioning, patching, and backups, allowing users to focus on apps rather than database management.

It achieves high performance by using a distributed architecture that separates compute and storage, allowing it to scale out horizontally across multiple nodes. It also provides instant scalability with auto-scaling capabilities, adjusting resources based on workload demand.

Compared to traditional relational database options like MySQL or PostgreSQL running on EC2 instances, Aurora offers **significantly higher performance, automatic failover, and continuous backup and replication across multiple Availability Zones, ensuring data durability and high availability.**

## RDS: Relational Database Service
A managed database service, like having a specialized filing cabinet for the structured data that takes care of organizing, retrieving, and storing efficiently.
RDS supports MySQL, PostgreSQL, MariaDB, Oracle, MS SQL Server, and Amazon Aurora.

Backup for better performance and avoid disaster.

RDS Proxy vs multi-AZ in terms of re-connecting to DB.s

- Read replica
	- RDS database can have up to 15 Read Replicas.
	- Read Replicas have Asynchronous Replication, therefore it's likely users will only read Eventual Consistency.

- RDS in multi-AZ
	- Multi-AZ keeps the same connection string regardless of which database is up.
	- Multi-AZ won't help when a disaster happens at the AWS region level. Multi-AZ helps when a disaster happens at the AZ level.

## **DynamoDB**: Fully managed NoSQL database.
DynamoDB is serverless with no servers to provision, patch, or manage and no software to install, maintain or operate. It auto scales tables up and down to adjust for capacity and maintain performance. It provides both provisioned (specify RCU & WCU) and on-demand (pay for what you use) capacity modes.

RCU stands for “Read Capacity Units,” and WCU stands for “Write Capacity Units." RCU and WCU are decoupled, so you can increase/decrease each value separately.

DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory **cache** for DynamoDB that delivers up to 10x performance improvement. It caches the most frequently used data, thus offloading the heavy reads on hot keys off your DynamoDB table, hence preventing the `ProvisionedThroughputExceededException` exception.

Amazon DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory cache for DynamoDB that delivers up to a 10x performance improvement – from milliseconds to microseconds – even at millions of requests per second. DAX does all the heavy lifting required to add in-memory acceleration to your DynamoDB tables, without requiring developers to manage cache invalidation, data population, or cluster management.

DynamoDB Streams allows you to capture a time-ordered sequence of item-level modifications in a DynamoDB table. It's integrated with AWS Lambda so that you create triggers that automatically respond to events in real-time.
DynamoDB Streams enable DynamoDB to get a changelog and use that changelog to replicate data across replica tables in other AWS Regions.

It has an out-of-the-box caching feature.

The maximum size of an item in a DynamoDB table is 400kb.

Amazon DynamoDB provides the on-demand backup capability to create full backups of your tables for long-term retention and archival for regulatory compliance needs.
## ElastiCache
ElastiCache automatically scales the cache cluster size based on the workload, handling varying levels of demand without manual intervention.

## Storage Extras
### Snowball Edge Storage Optimized device jobs
- AWS Snowball Edge is a **physical data transfer device** designed for **large-scale data migrations**. It allows for the **offline transfer** of large amounts of data from on-premises locations to AWS.

