**Keys:**
- Storage method
- I/O performance
- Availability
- Security
- Lifecycle
- Event notifications/log

## EFS: Elastic File System
File system that can 
- be mounted on many EC2 instances in multi-AZ
- allow multiple EC2 instances to access the file system at the same time
- content management, web serving, data sharing, wordpress.

NFS: Network File System, a distributed file system protocol that allows a user on a client computer to access files over a network as if they were stored locally on the client's own hard drive.
vs Google Drive
- serve similar purposes in providing file storage and access, but use different technologies and architectures. Google Drive is a cloud-based storage service with its own protocols, while NFS is a network file system protocol designed for sharing files over a local or wide area network. Google Drive is a cloud-based service designed for personal and collaborative file storage and sharing, while NFS is a network file system protocol primarily used for centralized file storage and sharing within traditional IT environments.

## S3: Simple Storage Service
An infinitely large **external hard drive** or cloud storage service to store and retrieve any amount of data, at any time, from anywhere on the web. Great for data backup, archiving, and serving web content.

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
	- **pre-signed URL v.s. public URL**
		- pre-signed URL contains a signature that carries the user's credentials, inherited from the permissions of the user that generated the URL. They are temporary URLs that you can grant time-limited access to some actions.
	- **Policies
		- Explicit `DENY` in an IAM Policy will take precedence over an S3 bucket policy.
		- **User-based:** IAM Policies, API calls should be allowed for a specific user from IAM.
		- **Resource-based**: grant access to buckets/accounts, encrypt objects at upload
			- Bucket Policies
				- JSON based policies: Resource, Effect, Actions, Principal
				- 
				- Use cases
			- Object ACL(Access Control List): finer grain
			- Bucket ACL: less common
		- Note: an IAM principal can access an S3 object if 
			- The user IAM permissions ALLOW it OR the resource policy ALLOWS it
			- AND there’s no explicit DENY

S3 File Gateway: a type of AWS Storage Gateway that extends on-premises file storage to the cloud, i.e. access and store files in S3 using standard file protocols such as NFS (Network File System) and SMB (Server Message Block).

With S3 File Gateway, the most recently accessed files can be cached locally for low-latency access, ensuring that users can quickly access frequently accessed files.


## **DynamoDB**: Fully managed NoSQL database.
DynamoDB is serverless with no servers to provision, patch, or manage and no software to install, maintain or operate. It auto scales tables up and down to adjust for capacity and maintain performance. It provides both provisioned (specify RCU & WCU) and on-demand (pay for what you use) capacity modes.

RCU stands for “Read Capacity Units,” and WCU stands for “Write Capacity Units." RCU and WCU are decoupled, so you can increase/decrease each value separately.

DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory **cache** for DynamoDB that delivers up to 10x performance improvement. It caches the most frequently used data, thus offloading the heavy reads on hot keys off your DynamoDB table, hence preventing the `ProvisionedThroughputExceededException` exception.

DynamoDB Streams allows you to capture a time-ordered sequence of item-level modifications in a DynamoDB table. It's integrated with AWS Lambda so that you create triggers that automatically respond to events in real-time.

The maximum size of an item in a DynamoDB table is 400kb.
## ElastiCache
ElastiCache automatically scales the cache cluster size based on the workload, handling varying levels of demand without manual intervention.

## Snowball Edge Storage Optimized device jobs
- AWS Snowball Edge is a **physical data transfer device** designed for **large-scale data migrations**. It allows for the **offline transfer** of large amounts of data from on-premises locations to AWS.