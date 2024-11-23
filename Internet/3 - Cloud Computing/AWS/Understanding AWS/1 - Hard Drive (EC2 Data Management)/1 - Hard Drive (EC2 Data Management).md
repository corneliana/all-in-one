[[1.1 - EBS - Elastic Block Store]]
[[1.2 - EC2 Instance Store]]
[[1.3 - AMI - Amazon Machine Image]]
[[1.4 - EFS - Elastic File System]]

Like a physical computer needs a hard drive to **store its operating system, applications, and data**, an EC2 instance requires a **virtual hard drive**. This is where EBS volumes come into play to provide **block storage volumes** for EC2 instances.

**Block-level storage**
- **What is block-level storage?**
	- Block: a fundamental unit of storage on storage devices (such as hard drives or SSDs). Each block has a fixed size and is independently accessible. With block-level storage like EBS or EC2 instance store, we're essentially **interacting directly with the storage device attached to the EC2 instance**. When I/O to specific blocks on the storage device, it is Operating System that handles these operations at the block level.
- **What's good about block-level storage? v.s. DataBase** 
	- It offers **fine-grained control over data storage and retrieval** at the expense of requiring more low-level management, while DB tools operate at a higher level of abstraction, providing built-in optimizations for common data operations.



