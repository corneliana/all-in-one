## Serverless
Serverless: don't need to manage/provision servers anymore, just deploy code and functions.
- in AWS
	- Lambda
		- For tasks that don't need a dedicated computer running all the time. It's like having robots wake up, perform a task quickly, and go back to sleep without worrying about the electricity bill.
	- DynamoDB
	- Cognito
	- API Gateway
		- A centralized server that manages, secures, and optimizes communication between clients and backend services in a microservices architecture.
	- S3
	- SNS & SQS
	- Kinesis Data Firehose
	- Aurora Serverless
	- Step Functions
	- Fargate

**Serverless Solution Architecture**
- TODO APP
	- REST API layer: API gateway => lambda => DynamoDB
	- giving user access to S3: use Amazon Cognito and then S3 with temporary credentials
	- high read throughput, static data: 
		- DAX as a caching layer between lambda and DynamoDB
		- Caching at the API Gateway

- Serverless hosted website: MyBlog.com
	- Server static content globally: CloudFront => S3
	- Server static content globally, securely: CloudFront => OAC => S3
	- Add a public serverless REST API: API gateway => lambda => DAX => DynamoDB Global Tables
	- User welcome email: DynamoBD Stream => invoke lambda(IAM role) => SES
	- Users upload image and thumbnail generated: upload => CloudFront(S3 transfer acceleration) => S3 => lambda => create and put thumbnail in another S3 => SQS/SNS

- Summary
	- We’ve seen static content being distributed using CloudFront with S3 • The REST API was serverless, didn’t need Cognito because public  
	- We leveraged a Global DynamoDB table to serve the data globally  
	- (we could have used Aurora Global Database)
	- We enabled DynamoDB streams to trigger a Lambda function  
	- The lambda function had an IAM role which could use SES  
	- SES (Simple Email Service) was used to send emails in a serverless way
	- S3 can trigger SQS / SNS / Lambda to notify of events

**Microservices architecture**
- You are free to design each micro-service the way you want
- Synchronous patterns: API Gateway, Load Balancers  
- Asynchronous patterns: SQS, Kinesis, SNS, Lambda triggers (S3)
- Challenges with micro-services:  
	- repeated overhead for creating each new microservice,  
	- issues with optimizing server density/utilization  
	- complexity of running multiple versions of multiple microservices simultaneously  
	- proliferation of client-side code requirements to integrate with many separate services.
- Some of the challenges are solved by Serverless patterns:  
	- API Gateway, Lambda scale automatically and you pay per usage  
	- You can easily clone API, reproduce environments  
	- Generated client SDK through Swagger integration for the API Gateway

**Software update distribution**
- Add CloudFront
	- No changes to architecture  
	- Will cache software update files at the edge  
	- Software update files are not dynamic, they’re static (never changing)
	- Our EC2 instances aren’t serverless  
	- But CloudFront is, and will scale for us  
	- Our ASG will not scale as much, and we’ll save tremendously in EC2
	- We’ll also save in availability, network bandwidth cost, etc  
	- Easy way to make an existing application more scalable and cheaper! if it's mostly static content just using some caching at the edges all around the world.

## Event Processing

**1. Lambda, SNS & SQS**
![[Pasted image 20240601120427.png]]

**2. Fan Out Pattern: deliver to multiple SQS**
![[Pasted image 20240601120501.png]]

**3. S3 Event Notifications**
![[Pasted image 20240601120521.png]]

**4. S3 Event Notifications with Amazon EventBridge**
![[Pasted image 20240601120538.png]]

**5. Amazon EventBridge – Intercept API Calls**
![[Pasted image 20240601120555.png]]

**6. API Gateway – AWS Service Integration Kinesis Data Streams example**
![[Pasted image 20240601120615.png]]

## Caching Strategies
![[Pasted image 20240601120734.png]]

## Blocking an IP Address
**1. Blocking an IP address**
![[Pasted image 20240601121526.png]]

**2. Blocking an IP address – with an ALB**
![[Pasted image 20240601121547.png]]

**3. Blocking an IP address – with an NLB**
![[Pasted image 20240601121556.png]]

**4. Blocking an IP address – ALB + WAF**
![[Pasted image 20240601121607.png]]

**5. Blocking an IP address – ALB, CloudFront WAF**
![[Pasted image 20240601121618.png]]

## High Performance Computing(HPC)
- The cloud is the perfect place to perform HPC
- You can create a very high number of resources in no time
- You can speed up time to results by adding more resources
- You can pay only for the systems you have used
- Perform genomics, computational chemistry, financial risk modeling, weather prediction, machine learning, deep learning, autonomous driving
- Which services help perform HPC?
	- Data Management & Transfer
		- AWS Direct Connect:  
			- Move GB/s of data to the cloud, over a private secure network
		- Snowball & Snowmobile  
			- Move PB of data to the cloud
		- AWS DataSync  
			- Move large amount of data between on-premises and S3, EFS, FSx for Windows
	
	- Compute and Networking
		- EC2 Instances:  
			- CPU optimized, GPU optimized  
			- Spot Instances / Spot Fleets for cost savings + Auto Scaling
		- EC2 Placement Groups: Cluster for good network performance
		- EC2 Enhanced Networking (SR-IOV)  
			- Higher bandwidth, higher PPS (packet per second), lower latency
				- Option 1: Elastic Network Adapter (ENA) up to 100 Gbps  
				- Option 2: Intel 82599 VF up to 10 Gbps – LEGACY
		- Elastic Fabric Adapter (EFA)
			- Improved ENA for HPC, only works for Linux
			- Great for inter-node communications, tightly coupled workloads
			- Leverages Message Passing Interface (MPI) standard
			- Bypasses the underlying Linux OS to provide low-latency, reliable transport

	- Storage
		- Instance-attached storage:  
			- EBS: scale up to 256,000 IOPS with io2 Block Express  
			- Instance Store: scale to millions of IOPS, linked to EC2 instance, low latency
		- Network storage:
			- Amazon S3: large blob, not a file system
			- Amazon EFS: scale IOPS based on total size, or use provisioned IOPS
			- Amazon FSx for Lustre:  
				- HPC optimized distributed file system, millions of IOPS
				- Backed by S3

	- Automation and Orchestration
		- AWS Batch
			- AWS Batch supports multi-node parallel jobs, which enables you to run single jobs that span multiple EC2 instances.
			- Easily schedule jobs and launch EC2 instances accordingly
		- AWS ParallelCluster
			- Open-source cluster management tool to deploy HPC on AWS
			- Configure with text files
			- Automate creation of VPC, Subnet, cluster type and instance types
			- Ability to enable EFA on the cluster (improves network performance)
	
## EC2 Instance High Availability
**1. Creating a highly available EC2 instance**
![[Pasted image 20240601121649.png]]

**2. Creating a highly available EC2 instance With an Auto Scaling Group**
![[Pasted image 20240601121705.png]]

**3. Creating a highly available EC2 instance With ASG + EBS**
![[Pasted image 20240601121718.png]]

