
## EC2: Elastic Compute Cloud = Infrastructure as a Service
An Amazon EC2 instance is like a virtual computer/server running in the cloud, similar to CPU and RAM of a computer.

- **Address => IP: Public / Private / Elastic** 
	- Private v.s. Public IP(IPv4): whether the machine can be identified on the public network
	- **Elastic IPs: a fixed public IPv4 IP for the instance.** 
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
	Golden AMI is an image that contains all your software installed and configured so that future EC2 instances can boot up quickly from that AMI.
	
- **Purchasing Options:** 
	- On-Demand: pay the full price for what you use at any time
	- Reserved: 1-3 year for a discount
	- Saving plans: pay a certain amount to a certain type and region on long-term usage
	- Spot: the highest bidder pays, i.e. your max price > current spot price. Most cost-efficient
	- Dedicated Hosts / Instance: EC2 instance capacity fully dedicated to your use
	- Capacity Reservations: Regional reserved instance + saving plans (pay full price for a period even you don't use it)
	- spot fleets = set of Spot instances + (optional) On-demand instances

ElasticIP: static IP addresses assigned to instances or resources in VPC. They can be easily associated or disassociated with instances, providing flexibility in managing IP addresses.

Spot Instances are indeed **less stable but cheaper** compared to On-Demand Instances. 
- Spot instances take advantage of unused EC2 capacity in the AWS cloud at a significantly lower price than On-Demand rates. However, this cost saving comes with the trade-off of reduced stability; **your instances can be terminated by AWS with only a two-minute notice if the demand for capacity increases or if someone else is willing to pay more**. This makes **Spot Instances ideal for workloads that are tolerant of interruptions and do not require continuous availability.**

**A fleet of EC2 instances**: refers to a collection or group of EC2 instances managed as a single entity to serve a specific purpose or workload. This term is often used in cloud computing to describe the way multiple virtual servers (instances) are used together to handle applications and services.

- **Lambda**: Serverless compute service for running code without provisioning or managing servers.