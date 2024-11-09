- Elastic
	- The ability to scale resources seamlessly and adapt to changing demands, whether it's storage capacity, performance, caching capacity, or IP address management.

- minimize operational overhead

- Endpoints v.s. Gateways
	- **Gateways** facilitate **general and controlled access between networks**, similar to border checkpoints managing entries and exits.
	- **Endpoints** provide **a specific, secure, and direct line to a service**, akin to having a private mailbox or dedicated delivery route that bypasses public traffic.

- compliance
	- the **adherence to various laws, policies, and regulations** when using AWS services to store, process, or transmit data. Compliance is crucial because it ensures that data handled by AWS is protected and managed in a way that meets specific standards and legal requirements.

- NFS or SMB protocol


- [[State]]
	- stateful v.s. stateless

- fully managed
	- E.g. Aurora, it means that AWS handles the heavy lifting of infrastructure management, but some level of user interaction and monitoring is still required to ensure the database operates effectively for the specific application needs.

- Storage v.s. memory
	- Storage is where data is kept long-term, and memory is where data is kept short-term for quick access by the computer's processor.

- **Region v.s. AZ**
	- Regions, like "ap-southeast," represent large geographic areas that contain multiple Availability Zones. Each Availability Zone within a region, such as "ap-southeast-1a," "ap-southeast-1b," etc., represents an isolated data center with its own infrastructure, facilities, and redundancy measures. Deploying resources across multiple Availability Zones within the same region ensures high availability and fault tolerance for applications by protecting against failures at the data center level.

- API Gateway v.s. Route 53
	- API Gateway is like a bouncer at a club, managing access to your backend services and APIs, while Route 53 is like a GPS for the internet, directing traffic to your web applications using domain names.

- EventBridge events v.s. CloudWatch alarms
	- AWS EventBridge events are like notifications about things happening in your AWS environment, while CloudWatch alarms are like alerts that trigger based on specific conditions you define. EventBridge events are broader and can trigger actions based on various events, while CloudWatch alarms focus on monitoring specific metrics and triggering alerts when thresholds are exceeded.

- Data warehouse: 让大量数据便于管理
	- Think of data warehousing as a big organized storage unit for your data. In the context of AWS, data warehousing means storing and managing large volumes of data in a way that makes it easy to analyze and extract insights from.

- SQS v.s. SNS
	- both SQS and SNS facilitate messaging in distributed systems, SQS is suited for point-to-point communication and work distribution among consumers, whereas SNS is designed for broadcasting messages to multiple subscribers or endpoints.

HDD v.s. SSD

AWS Global Accelerator and Amazon CloudFront are separate services that use the AWS global network and its edge locations around the world. CloudFront improves performance for both cacheable content (such as images and videos) and dynamic content (such as API acceleration and dynamic site delivery), while Global Accelerator improves performance for a wide range of applications over TCP or UDP.


# [[OSI (Open Systems Interconnection) model]]
Here it involves the 7 layers of Internet.
1. **Physical Layer (OSI Layer 1)**: AWS Direct Connect, establishes a dedicated network connection between AWS and an on-premises data center, bypassing the public internet.
2. **Data Link Layer (OSI Layer 2)**: N/A
3. **Network Layer (OSI Layer 3)**: 
	- VPC: provide a logically isolated section to launch AWS resources in a virtual network.
	- Route 53: DNS web service for routing end users to Internet applications.
4. **Transport Layer (OSI Layer 4)**:
    - Global Accelerator: Improves availability and performance of apps with local and global traffic management.
	- Network Load Balancer: Distributes incoming application traffic across multiple targets, such as EC2 instances.
5. **Session Layer (OSI Layer 5)**: N/A
6. **Presentation Layer (OSI Layer 6)**: N/A
7. **Application Layer (OSI Layer 7)**:
    - Compute Services:
	    - EC2: Provides resizable compute capacity in the cloud.
	    - Lambda: Runs code without provisioning or managing servers.
	- Storage Services:
	    - S3: Object storage service designed to store and retrieve any amount of data.
	    - EBS: Provides block-level storage volumes for use with EC2 instances.
	- Database Services:
	    - RDS: Managed relational database service supporting various database engines like MySQL, PostgreSQL, etc.
	    - DynamoDB: Fully managed NoSQL database service.
	- Networking Services:
	    - API Gateway: Fully managed service for creating, publishing, maintaining, monitoring, and securing APIs at any scale.
	    - CloudFront: CDN for fast and secure delivery of data, videos, apps, and APIs.
	    - ELB: Automatically distributes incoming application traffic across multiple targets.
	    - Route 53: Scalable DNS web service for routing end users to Internet applications.

