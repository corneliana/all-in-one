AWS: Amazon Web Services, a cloud computing platform.

So what is cloud computing?
Cloud computing is a new layer of abstraction over physical computing resources, which transforms them into scalable, on-demand services that can be used without managing the underlying hardware directly.

The platform is used in a specific region => **Availability Zones (AZ)**
- global services: IAM, Route 53(DNS Service), CloudFront(Content Delivery Network), WAF(Web Application Firewall)
- regional services: EC2(IaaS), Elastic Beanstalk(PaaS), Lambda(FaaS), Rekognition(SaaS)

## IAM: Identity and Access Management
- Create: 
	- root account: created by default
	- user
	- group
- Protect/Security: 
	- the lest privilege principle
	- 

storage is where data is kept long-term, and memory is where data is kept short-term for quick access by the computer's processor.


## EC2: Elastic Compute Cloud = Infrastructure as a Service
An Amazon EC2 instance is like a virtual computer running in the cloud.

## EBS
Just like a physical computer needs a hard drive to store its operating system, applications, and data, an EC2 instance requires a virtual hard drive. This is where EBS volumes come into play.

EBS volumes are like the live, working documents on computer, while snapshots are like saved versions of these documents at specific points in time, useful for backup or creating duplicates.

An **EBS Volume** is a durable, block-level storage device that can be attached to a single EC2 instance. It's essentially your virtual hard drive.

An **EBS Snapshot** is a point-in-time copy of an EBS Volume. Snapshots are stored in Amazon S3 and provide a historical record of a volume's state.

A virtual hard drive (EBS volume) for EC2 instance ensures persistent, flexible, and high-performance storage for applications, while snapshots provide a robust backup and disaster recovery solution so as to safeguard and replicate data as needed.

## ELB

## ASG

## RDS

## Aurora

## Elastic Cache





