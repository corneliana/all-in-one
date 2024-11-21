[[4.1 - EFS]]
[[4.2 - S3 Simple Storage Service]]
[[4.3 - Aurora]]
[[4.4 - RDS Relational Database Service]]
[[4.5 - DynamoDB]]
[[4.6 - ElastiCache]]
[[4.7 - Storage Extras]]

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
