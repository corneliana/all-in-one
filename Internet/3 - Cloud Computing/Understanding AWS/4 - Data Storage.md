**Keys:**
- Storage: how much data and for how long? Will it grow? Average object size? How are they accessed? Data durability? Source of truth for the data?
- Performance: I/O and throughput needs? Will it change, does it need to scale or fluctuate during the day?
- Availability: Latency requirements? Concurrent users?
- Security
- Lifecycle
- Event notifications/log

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
		- One Zone: **One AZ, great for dev, backup enabled by default, compatible with IA (EFS One Zone-IA)**

NFS: Network File System, a distributed file system protocol that allows a user on a client computer to access files over a network as if they were stored locally on the client's own hard drive.
vs Google Drive
- serve similar purposes in providing file storage and access, but use different technologies and architectures. Google Drive is a cloud-based storage service with its own protocols, while NFS is a network file system protocol designed for sharing files over a local or wide area network. Google Drive is a cloud-based service designed for personal and collaborative file storage and sharing, while NFS is a network file system protocol primarily used for centralized file storage and sharing within traditional IT environments.

## S3: Simple Storage Service
An **infinitely scaling storage service** to store and retrieve any amount of data, at any time, from anywhere on the web.

**Use cases:** Backup and storage, Disaster Recovery, Archive, Hybrid Cloud Storage, Application Hosting, Media Hosting, Data Lakes & big data analytics, Software delivery, Static website

When you use bucket default settings, you don't specify a Retain Until Date. Instead, you specify a **duration**, in either days or years, for which every object version placed in the bucket should be protected.

- **Buckets** (dictionaries) to store objects (files) **at regional level**.
- **Versioning** files at bucket level. Must enable versioning in source and destination buckets. Once you version-enable a bucket, it can never return to an unversioned state. Versioning can only be suspended once it has been enabled.
- **Replication** enables **automatic, asynchronous copying of objects across buckets.**
	- Cross-region replication
- **Services provided: Static website hosting**
	- S3 can host static websites and have them accessible on the Internet. If 403, makes sure the bucket policy allows public reads.
- **Lifecycle**: move between storage classes or using S3 Lifecycle configurations
	- ![[s3-storage-classes.png]]

- **Performance:**
	- **Store
		- **Baseline**
			- Amazon S3 automatically scales to high request rates, latency 100-200 ms
			- Your app can **achieve at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix in a bucket.**
			- There are no limits to the number of prefixes in a bucket. You can increase I/O performance by parallelizing reads. **For example, 10 prefixes in S3 bucket to parallelizing reads, can scale read performance to 55,000 read requests per second.**
		- **Multi-Part upload: big files divided into parts**, if uploading more than 5GB.
		- **Transfer acceleration** 
			- **transfer files to AWS Edge Location(part of CloudFront)** that are are optimized for fast content delivery by leveraging AWS's global network infrastructure, routing data through Edge Locations closest to the uploader.
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
	- **Policies: define the inbound and outbound traffic for a bucket
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
- Event notifications: CRUD operations on the objects in S3
	- Events => S3 => SNS / SQS / Lambda
	- Events => EventBridge (Advanced filtering options with JSON rules) => over 18 services as destinations
	- IAM Permissions: Resource access policy
- S3 lambda
	- Use lambda to change the object before it is being retrieved.
	- E.g. redact PII data, convert data format, resize and watermark images, etc.
- **S3 File Gateway**: a type of AWS Storage Gateway that **extends on-premises file storage to the cloud**, i.e. access and store files in S3 using standard file protocols such as **NFS (Network File System) and SMB (Server Message Block**). With S3 File Gateway, the most recently accessed files can be cached locally for low-latency access, ensuring that users can quickly access frequently accessed files.
- **Payment**
	- In general, bucket owner pays for all the storage and data transfer costs associated with the bucket.
	- With requester pays bucket, the requester pays the cost of the request and the data downloaded from the bucket. Helpful when share large datasets with other accounts.

## Aurora
**MySQL and PostgreSQL-compatible relational database** that offers the performance and availability of commercial-grade databases at little cost.
It is **fully managed** by AWS, meaning AWS handles administrative tasks such as provisioning, patching, and backups, allowing users to focus on apps rather than database management.

It achieves high performance by using a distributed architecture that separates compute and storage, allowing it to scale out horizontally across multiple nodes. It also provides instant scalability with auto-scaling capabilities, adjusting resources based on workload demand.

Compared to traditional relational database options like MySQL or PostgreSQL running on EC2 instances, Aurora offers **significantly higher performance, automatic failover, and continuous backup and replication across multiple Availability Zones, ensuring data durability and high availability.**

Up to 15 Aurora Read Replicas in a single Aurora DB Cluster.

## RDS: Relational Database Service
A **managed** database service, like having a specialized filing cabinet for the structured data that takes care of organizing, retrieving, and storing efficiently. **RDS supports MySQL, PostgreSQL, MariaDB, Oracle, MS SQL Server, and Amazon Aurora.**

RDS Proxy vs multi-AZ in terms of re-connecting to DB.

Multi-AZ follows **synchronous** replication and spans at least two Availability Zones (AZs) within a single region. Read replicas follow **asynchronous** replication and can be within an Availability Zone (AZ), Cross-AZ, or Cross-Region

- Read replica
	- RDS database can have up to 15 Read Replicas.
	- Read Replicas have Asynchronous Replication, therefore it's likely users will only read Eventual Consistency.

- RDS in multi-AZ
	- Multi-AZ keeps the same connection string regardless of which database is up.
	- Multi-AZ won't help when a disaster happens at the AWS region level. Multi-AZ helps when a disaster happens at the AZ level.

- Encryption
	- Encrypt an unencrypted RDS DB instance: Create a snapshot of the unencrypted RDS DB instance, copy the snapshot and tick "Enable encryption", then restore the RDS DB instance from the encrypted snapshot.
	- Can not create encrypted Read Replicas from an unencrypted RDS DB instance.
	- Oracle does **NOT** support IAM Database Authentication

## **DynamoDB**: Fully managed NoSQL database.
- A NoSQL database service for unstructured data, like a magical notebook that instantly stores and retrieves notes no matter how many you have.
- **serverless** with no servers to provision, patch, or manage and no software to install, maintain or operate. 
- auto scales tables up and down to adjust for capacity and maintain performance. Also provides the on-demand backup capability to create full backups of tables for long-term retention and archival for regulatory compliance needs.
- provides both provisioned (specify RCU & WCU) and on-demand (pay for what you use) capacity modes. RCU = “Read Capacity Units,” and WCU = “Write Capacity Units." RCU and WCU are decoupled, so you can increase/decrease each value separately.
- The maximum size of an item in a DynamoDB table is **400kb**.

- DAX: DynamoDB Accelerator
	- A fully managed, highly available, in-memory cache for DynamoDB that delivers up to a 10x performance improvement without requiring developers to manage cache invalidation, data population, or cluster management. It caches the most frequently used data, thus offloading the heavy reads on hot keys off your DynamoDB table, hence preventing the `ProvisionedThroughputExceededException` exception.

- DynamoDB Streams
	- It allows you to capture a time-ordered sequence of item-level modifications in a DynamoDB table. It's integrated with AWS Lambda so that you create triggers that automatically respond to events in real-time.
	- It enable DynamoDB to get a changelog and use that changelog to replicate data across replica tables in other AWS Regions.

It has an out-of-the-box caching feature.

## ElastiCache
A fully managed, in-memory caching service. "elastic" refers to the service's ability to automatically scale resources based on demand, allowing apps to efficiently handle varying workloads. It provides managed **Redis** and **Memcached** solutions to help devs to easily deploy and scale caching infrastructure in the cloud.
- Redis: offers advanced features and **data structures**, suitable for a wide range of use cases
	- An open-source, in-memory data **structure** store that can be used as a database, cache, or message broker. It supports various data structures such as strings, hashes, lists, sets, and sorted sets, making it versatile for a wide range of use cases. 
	- Known for its **advanced features such as persistence, replication, pub/sub messaging, and built-in Lua scripting capabilities.**
	- Commonly used for caching frequently accessed data, session storage, real-time analytics, job queues, and more.
- Memcached: **focuses primarily on high-performance caching.**
	- Store data in memory as key-value pairs, allowing for fast retrieval of frequently accessed data.
	- A high-performance, distributed memory object caching system, designed to speed up dynamic web applications by alleviating database load and reducing latency.
	- Unlike Redis, Memcached is simpler in terms of functionality and data types supported. It primarily focuses on caching and does not offer persistence or advanced data structures.
	- Memcached is commonly used for caching database query results, HTML fragments, and API responses in web applications.

## Storage Extras
### Snow Family: offline devices to process data at the edge and migrate data into and out of AWS
If it takes more than a week to transfer over the network, use Snowball devices **(physical route, not network route)**.

Highly-secure, portable devices to 
- either collect and process data at the edge
- or migrate data into and out of AWS

- Data migration
	- Snowcone
		- Use Snowcone where Snowball doesn't fit (space-constrained env)
		- Must provide your own battery / cables
		- Can be sent back to AWS offline, or connect it to internet and use AWS DataSync to send data
	- **Snowball Edge**
		- AWS Snowball Edge is a **physical data transfer device** designed for **large-scale data migrations** without consuming excessive network bandwidth. It allows for the **offline transfer** of large amounts of data from on-premises locations to AWS.
		- Comes with **computing capabilities** and allows you to pre-process the data while it's being moved into Snowball.
		- Use cases: large data cloud migrations, DC decommissions, disaster recovery 
	- Snowmobile: an actual truck
		- Better use case than Snowball if transfer more than 10PB
![[Snow-family-for-data-migrations.png]]

- Usage process
	- Request Snowball devices from the AWS console for delivery
	- Install the snowball client / AWS OpsHub on your servers
	- Connect the snowball to your servers and copy files using the client
	- Ship back the device when you’re done (goes to the right AWS facility)
	- Data will be loaded into an S3 bucket
	- Snowball is completely wiped

- Edge computing
	- Process data while it’s being created on an edge location
	- These locations may have
		- **Limited / no internet access**
		- **Limited / no easy access to computing power**
	- We setup a **Snowball Edge / Snowcone device** to do edge computing
		- Snowcone & Snowcone SSD (smaller)  
			- 2 CPUs, 4 GB of memory, wired or wireless access • USB-C power using a cord or the optional battery
		- Snowball Edge – **Compute Optimized**
			- 104 vCPUs, 416 GiB of RAM
			- Optional GPU **(useful for video processing or machine learning)**
			- 28 TB NVMe or 42TB HDD usable storage  
			- Storage Clustering available (up to 16 nodes)
		- Snowball Edge – Storage Optimized  
			- Up to 40 vCPUs, 80 GiB of RAM, 80TB storage
		- All: **Can run EC2 Instances & AWS Lambda functions (using AWS IoT Greengrass)** 
		- Long-term deployment options: **1 and 3 years discounted pricing**
	- Use cases
		- Preprocess data
		- Machine learning at the edge
		- Transcoding media streams
	- Eventually (if need be) we can ship back the device to AWS (for transferring data for example)
	- AWS OpsHub: a CLI (Command Line Interface tool) to use Snow Family devices
		- Unlocking and configuring single or clustered devices
		- Transferring files
		- Launching and managing instances running on Snow Family Devices
		- Monitor device metrics (storage capacity, active instances on your device)
		- Launch compatible AWS services on your devices (ex: Amazon EC2 instances, AWS DataSync, Network File System (NFS))

### Architecture: Snowball into Glacier
- Snowball cannot import to Glacier directly  
- You must **use Amazon S3 first**, in combination with an S3 lifecycle policy
![[snowball-into-glacier.png]]

### Amazon FSx
**fully managed service** to launch 3rd party high-performance file systems on AWS
- FSx for Windows
	- FSx for Windows is **a fully managed Windows file system share drive**
	- Suppor ts SMB protocol & Windows NTFS  
	- **Microsoft Active Directory integration, ACLs, userquotas**
	- has integration with Microsoft **Active Directory** for access control
	- Can be mounted on Linux EC2 instances
	- Supports Microsoft's **Distributed File System (DFS)** Namespaces (group files across multiple FS)
	- Scale up to 10s of GB/s, millions of IOPS, 100s PB of data
	- Storage Options:  
		- SSD – latency sensitive workloads (db, media processing, data analytics, ...) 
		- HDD – broad spectrum of workloads (home directory, CMS, ...)
	- **Can be accessed from your on-premises infrastructure (VPN or Direct Connect)**
	- Can be configured to be **Multi-AZ (high availability) ** 
	- **Data is backed-up daily to S3**

- FSx for Lustre
	- Lustre is a type of **parallel distributed file system, for large-scale computing**
	- The name Lustre is derived from **“Linux” and “cluster"**
	- **Machine Learning, High Performance Computing (HPC)**
	- **Video Processing, Financial Modeling, Electronic Design Automation**
	- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies
	- Storage Options:  
		- SSD – low-latency, IOPS intensive workloads, small & random file operations • HDD – throughput-intensive workloads, large & sequential file operations
	- Seamless integration with S3
	- Can “read S3” as a file system (through FSx) 
	- Can write the output of the computations back to S3 (through FSx)
	- Can be used from on-premises servers (VPN or Direct Connect)
	- File System Deployment Options
		- Scratch File System
			- Temporary storage
			- Data is not replicated (doesn’t persist if file server fails)
			- High burst (6x faster, 200MBps per TiB)
			- Usage: short-term processing, optimize costs
		- Persistent File System
			- Long-term storage
			- Data is replicated within same AZ
			- Replace failed files within minutes
			- Usage: long-term processing, sensitive data

- FSx for NetApp ONTAP
	- Managed NetApp ONTAP on AWS
	- File System compatible with **NFS, SMB, iSCSI protocol**
	- Move workloads running on ONTAP or NAS to AWS
	- Works with: 
		- Linux  
	    - Windows
		- MacOS
	    - VMware Cloud on AWS
	    - Amazon Workspaces & AppStream 2.0
	    - Amazon EC2, ECS and EKS
	- Storage shrinks or grows automatically
	- Snapshots, replication, low-cost, compression and data
	- Point-in-time instantaneous cloning (helpful for testing new workloads)

- Amazon FSx for Tape Gateway
	- Managed NetApp ONTAP on AWS
	- File System compatible with **NFS, SMB, iSCSI protocol**
	- Move workloads running on ONTAP or NAS to AWS
	- Works with:
		- Linux
	    - Windows
	    - MacOS
	    - VMware Cloud on AWS
	    - Amazon Workspaces & AppStream 2.0
	    - Amazon EC2, ECS and EKS
	- Storage shrinks or grows automatically
	- Snapshots, replication, low-cost, compression and data
	- Point-in-time instantaneous cloning (helpful for testing new workloads)

### Storage Gateway
- Hybrid Cloud for Storage
	- AWS is pushing for ”hybrid cloud”  
	    - Part of your infrastructure is on the cloud
	    - Part of your infrastructure is on-premises
	- This can be due to  
	    - Long cloud migrations  
	    - Security requirements  
	    - Compliance requirements
	    - IT strategy
	- S3 is a **proprietary storage technology (unlike EFS / NFS)**, so how do you expose the S3 data on-premises?
	- AWS Storage Gateway!
![[AWS-strorage-cloud-native-options.png]]
- AWS Storage Gateway
	- Bridge between on-premises data and cloud data
	- Use cases:  
		- disaster recovery
		- backup & restore  
		- tiered storage  
		- on-premises cache & low-latency files access
	- Types of Storage Gateway:
		- S3 File Gateway
			- Configured S3 buckets are accessible using the NFS and SMB protocol  
			- Most recently used data is cached in the file gateway  
			- SupportsS3Standard, S3StandardIA, S3OneZoneA, S3IntelligentTiering  
			- **Transition to S3 Glacier using a *Lifecycle Policy***
			- Bucket access using IAM roles for each File Gateway  
			- SMB Protocol has integration with **Active Directory (AD)** for user authentication
		- FSx File Gateway
			- Native access to Amazon FSx for Windows File Server  
			- **Local cache** for frequently accessed data  
			- Windows native compatibility (SMB, NTFS, Active Directory...)
			- Useful for group file shares and home directories
		- Volume Gateway
			- Block storage using iSCSI protocol backed by S3  
			- Backed by EBS snapshots which can help restore on-premises volumes!
			- Cached volumes: low latency access to most recent data  
			- Stored volumes: entire dataset is on premise, scheduled backups to S3
		- Tape Gateway
			- Some companies have backup processes using physical tapes (!)  
			- With Tape Gateway, companies use the same processes but, in the cloud 
			- VirtualTape Library (VTL) backed by Amazon S3 and Glacier  
			- Back up data using existing tape-based processes (and iSCSI interface)  
			- Works with leading backup software vendors

Storage Gateway – Hardware appliance
- Using Storage Gateway means you need on-premises virtualization
- Otherwise, you can use a Storage Gateway Hardware Appliance
- You can buy it on amazon.com
- Works with File Gateway,Volume Gateway,
- Has the required CPU, memory, network, SSD cache resources
- Helpful for daily NFS backups in small data centers

![[AWS-storage-gateway.png]]

### Transfer family
- **A fully-managed service** for file transfers **into and out of S3 or EFS** using **FTP protocol**
- Supported Protocols  
    - AWS Transfer for **FTP (File Transfer Protocol (FTP)) ** 
    - AWS Transfer for **FTPS (File Transfer Protocol over SSL (FTPS))**
    - AWS Transfer for **SFTP (Secure File Transfer Protocol (SFTP))**
- Managed infrastructure, Scalable, Reliable, Highly Available (multi-AZ)
- Pay per provisioned endpoint per hour + data transfers in GB
- Store and manage users’ credentials within the service
- **Integrate with existing authentication systems (Microsoft Active Directory, LDAP, Okta, Amazon Cognito, custom)**
- Usage: sharing files, public datasets, CRM, ERP, ...
![[AWS-transfer-family.png]]

### DataSync
- Move large amount of data to and from  
	- On-premises / other cloud to AWS (NFS, SMB, HDFS, S3 API...) – **needs agent**
	- AWS to AWS (different storage services) – no agent needed
- Can synchronize to:  
	- Amazon S3 (any storage classes – including Glacier)
	- Amazon EFS  
	- Amazon FSx (Windows, Lustre, NetApp, OpenZFS...)
- Replication tasks can be scheduled hourly, daily, weekly  
	- File permissions and metadata are preserved (NFS POSIX, SMB...)
	- One agent task can use 10 Gbps, can setup a bandwidth limit
	![[AWS-Datasync.png]]
- Want to use DataSync, but don't have the network capacity to do so
		=> use AWS Snowcone that comes with the DataSync agent pre-installed on it
		=> So run Snowcone on-premises, pull the data, run DataSync agents, then it will be shipped back into AWS region
	 ![[AWS-DataSync-between-aws-resources.png]]	
## Storage Comparison
- S3: Object Storage  
- S3 Glacier: ObjectArchival  
- EBS volumes: Network storage for one EC2 instance at a time  
- Instance Storage: Physical storage for your EC2 instance (high IOPS)  
- EFS: Network File System for Linux instances, POSIX filesystem  
- FSx for Windows: Network File System for Windows servers  
- FSx for Lustre: High Performance Computing Linux file system  
- FSx for NetApp ONTAP: High OS Compatibility  
- FSx for OpenZFS: Managed ZFS file system  
- Storage Gateway: S3 & FSx File Gateway,Volume Gateway (cache & stored),Tape Gateway • Transfer Family: FTP, FTPS, SFTP interface on top of Amazon S3 or Amazon EFS  
- DataSync: Schedule data sync from on-premises to AWS, or AWS to AWS  
- Snowcone / Snowball / Snowmobile: to move large amount of data to the cloud, physically
- Database: for specific workloads, usually with indexing and querying

### Use Cases
- **Snowball**: Best for **large-scale data migration when limited by internet bandwidth**, such as moving an entire data center to AWS.
- **Storage Gateway**: Ideal for **hybrid cloud storage solutions**, allowing organizations to **extend their existing on-premises storage to the cloud** for backup, disaster recovery, or additional capacity.
- **Transfer Family**: Suitable for **businesses that need to maintain legacy file transfer protocols but want to leverage the cloud for storage**, such as media companies sending large files.
- **DataSync**: Useful for **continuous, scheduled, or one-time data migration and sync needs**, **particularly when speed and data verification are critical, such as daily backups or active data archiving**.
