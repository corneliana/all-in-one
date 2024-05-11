**Keys:**
- Storage method
- I/O performance
- Availability
- Security
- Lifecycle
- Event notifications/log

## S3: Simple Storage Service

Think of S3 as:
- an infinitely large external hard drive
- or cloud storage service to store and retrieve any amount of data, at any time, from anywhere on the web.
It's great for data backup, archiving, and serving web content.

**Buckets** (dictionaries) to store objects (files) at regional level.
**Versioning** files at bucket level. Must enable versioning in source and destination buckets.
**Replication** enables automatic, asynchronous copying of objects across buckets.
Cross-region replication
**Lifecycle**: move between storage classes or using S3 Lifecycle configurations

**Performance:**
- **Store
	- Baseline
	- Multi-Part upload: big files divided into parts
	- **Transfer acceleration** 
		- transfer files to AWS Edge Location(part of CloudFront), which are are optimized for fast content delivery by leveraging AWS's global network infrastructure, routing data through Edge Locations closest to the uploader.
		- then forward file to the S3 bucket in the target region within the AWS network backbone.
- **Read
	- S3 Byte-Range Fetches
	- S3 Select & Glacier Select
- **Batch Operations


## Snowball Edge Storage Optimized device jobs
