Like a physical computer needs a hard drive to **store its operating system, applications, and data**, an EC2 instance requires a **virtual hard drive**. This is where EBS volumes come into play to provide **block storage volumes** for EC2 instances.

**Block-level storage**
- **What is block-level storage?**
	- Block: a fundamental unit of storage on storage devices (such as hard drives or SSDs). Each block has a fixed size and is independently accessible. With block-level storage like EBS or EC2 instance store, we're essentially **interacting directly with the storage device attached to the EC2 instance**. When I/O to specific blocks on the storage device, it is Operating System that handles these operations at the block level.
- **What's good about block-level storage? v.s. DataBase** 
	- It offers **fine-grained control over data storage and retrieval** at the expense of requiring more low-level management, while DB tools operate at a higher level of abstraction, providing built-in optimizations for common data operations.

## EBS: Elastic Block Store
- **What is EBS? or EBS volume?**
	- A network drive that instances are attached while they run(i.e. not a physical drive)
		- It uses the network to communicate the instance, so there might be a bit of latency
		- It can be detached from an EC2 instance and attached to another one quickly
		- Deleted on EC2 termination, so preserve root volume when instance terminates
	- Locked to an Availability Zone (AZ)
		- An EBS Volume in us-east-1a cannot be attached to us-east-1b
		- To move a volume across, snapshot it first
	- Have a provisioned capacity (size in GBs, and IOPS)
		- Get billed for all the provisioned capacity
		- Can increase the capacity of the drive over time

- **Snapshot: a backup of volume at a point of time**
	- Snapshot can be copied across AZ or Region, stored in S3, providing a historical record of volume states for backup, disaster recovery, and data protection purposes.
	- Archive: Move snapshot to **archive tier** that is 75% cheaper. Takes 24 to 72 hours for restoring the archive.
	- Recycle Bin: Set up rules to retain deleted snapshots so they can be recovered.
	- Fast snapshot restore(FSR): Force full initialization to have no latency on the first use.

- **EBS Volume Types*
	- **General Purpose SSD:** cost-effective usage, **system boot volumes, virtual backups, development and test environments**. gp2/gp3
	- **Provisioned IOPS(PIOPS) SSD:** sustained IOPS performance(6000), database workloads. **Supports EBS Multi-attach**. io1/io2, io3 Block Express.
	- **Hard Disk Driv
	- es(HDD): Cannot be a boot volume**
		- Throughput Optimized HDD(stl): big data, data warehouse, log processing.
		- Cold HDD(scl): for data that is frequently accessed, lowest cost.
	- **Only gp2/gp3 and io1/io2 can be used as boot volumes**
	
- **EBS Multi-attach:** can attach one or more volumes to an EC2 instance in the same AZ
	- These volumes provide storage for the instance, operating system, apps and data. Each instance has full read/write permissions. **Up to 16 EC2 instances at a time.** 
	- Each volume is independent of the EC2 instance and can be attached/detached from different instances as needed. It retains its data even when detached from an instance.
	- Achieve higher application availability in clustered Linux applications.
	- Application must manage concurrent write operations.

- **EBS encryption**
	- data inside the volume
	- data in flight between the instance and volume
	- All snapshots and volumes created from the snapshot. So to encrypt an unencrypted volume, create a snapshot of the volume, encrypt the snapshot and create new volume from the snapshot. Now can attach the encrypted volume to the original instance.

## EC2 Instance Store
Provides temporary block-level storage for EC2 instances.

higher-performance hardware disk than EBS
- Better I/O performance
- Lose storage if stopped(ephemeral)
- Good for buffer/cache/scratch data/temporary content
- Risk of data loss if hardware fails
- Backups and replication are your responsibility
