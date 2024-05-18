Like a physical computer needs a hard drive to **store its operating system, applications, and data**, an EC2 instance requires a **virtual hard drive**. This is where EBS volumes come into play to provide block storage volumes for EC2 instances.

- Block: a fundamental unit of storage on storage devices (such as hard drives or SSDs). Each block has a fixed size and is independently accessible. With block-level storage like EBS or EC2 instance store, we're essentially **interacting directly with the storage device attached to the EC2 instance**. You can I/O to specific blocks on the storage device, and the operating system handles managing these operations at the block level.
- Block-level storage offers **fine-grained control over data storage** and retrieval at the expense of requiring more low-level management, while database tools operate at a higher level of abstraction, providing built-in optimizations for common data operations.

Note: Storage is where data is kept long-term, and memory is where data is kept short-term for quick access by the computer's processor.

## EBS: Elastic Block Store
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


## EC2 Instance Store
Provides temporary block-level storage for EC2 instances.

higher-performance hardware disk than EBS
- Better I/O performance
- Lose storage if stopped(ephemeral)
- Good for buffer/cache/scratch data/temporary content
- Risk of data loss if hardware fails
- Backups and replication are your responsibility