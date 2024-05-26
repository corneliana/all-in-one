## ELB: Elastic Load Balancing
Distributes incoming application traffic across multiple targets.
The `smart mail sorting center` for directing internet traffic to different servers.
- Only NLB provides **both static DNS name and static IP**. 
- ALB provides a static DNS name but it does NOT provide a static IP. The reason being that AWS wants ELB to be accessible using a static endpoint, even if the underlying infrastructure that AWS manages changes.

#### Stickiness / Session affinity
ELB Sticky Session feature ensures traffic for the same client is always redirected to the same target (e.g., EC2 instance). This helps that the client does not lose his session data.

The following cookie names are reserved by the ELB (AWSALB, AWSALBAPP, AWSALBTG).

When using an ALB to distribute traffic to EC2 instances, the IP address that receives requests from will be the ALB's private IP addresses. To get the client's IP address, ALB adds an additional header called "X-Forwarded-For" contains the client's IP address.
Application Load Balancers support HTTP, HTTPS and WebSocket.
ALBs can route traffic to different Target Groups based on URL Path, Hostname, HTTP Headers, and Query Strings.

NLB provides the highest performance and lowest latency if the app needs it.
NLB has one static IP address per AZ and you can attach an Elastic IP address to it. ALB and Classic Load Balancers have a static DNS name.
NLB supports HTTP health checks as well as TCP and HTTPS

Server Name Indication (SNI) allows you to expose multiple HTTPS applications each with its own SSL certificate on the same listener. Read more here: https://aws.amazon.com/blogs/aws/new-application-load-balancer-sni/

## ASG: Auto Scaling Group
Automatically adjusts the number of EC2 instances in response to demand.
- Can't go over the maximum capacity (you configured) during scale-out events.
- When an EC2 instance fails the ALB Health Checks, it is marked unhealthy and will be terminated while the ASG launches a new EC2 instance.

## CloudFront: AWS's Content Delivery Network (CDN)
It accelerates the delivery of websites, APIs, video content, and other web assets by caching them at edge locations around the world. When users request content, CloudFront delivers it from the nearest edge location, reducing latency and improving performance. 
- Cache
	- Improve read performance, content is cached at the edge(the outer reaches of the global network). When a user requests content, CloudFront checks its cache to see if the content is already stored at an edge location near the user. If the content is not in the cache or if it has expired, CloudFront retrieves the content from the origin server specified for that particular distribution.
- 216 Point of Presence(PoP) globally => Global Edge network
- DDoS protection(because worldwide), integration with Shield, AWS web app firewall.

- Edge Locations
	- Strategically distributed around the world, often located in or near major cities and Internet exchange points.
	- Leverage Amazon's highly redundant and low-latency network infrastructure, allowing for efficient data transfer. 
	- Cache frequently accessed content, further reducing latency for subsequent requests.
	- Lambda@Edge is a feature of CloudFront that run code closer to users, which improves performance and reduces latency.
- Origins
	- The origin servers or locations where CloudFront retrieves content to serve to end users
	- Can be various types of **servers or storage services**, such as S3 buckets, EC2 instances, ELB, or custom HTTP servers.
		- S3
			- distribute files and cache them at the edge
			- Enhanced security with Origin Access Control(OAC), replacing OAI(Identity)
			- CloudFront v.s. S3 Cross Region Replications
				- CloudFront great for static content that must be available everywhere
				- S3 Cross Region Replication: Great for dynamic content that needs to be available at low-latency in few regions.
		- **Custom Origin(HTTP)
			- ALB
			- EC2 instance
			- S3 website
			- Any HTTP backend
	- Cache and distribute content globally across its network of edge locations.
	- Determines where CloudFront fetches content from and how it delivers that content to users efficiently.
- **Geo Restrictions
	- Allowlist & Blocklist

Key features of CloudFront include:
1. **Global Content Delivery**: operates from a network of edge locations around the world, enabling it to deliver content with low latency to users globally.
2. **Caching**: caches copies of content at edge locations, reducing the need to fetch it from origin server for subsequent requests to lower latency and reduce the load on origin server.
3. **Security**: provides various security features, including SSL/TLS encryption, AWS Shield protection against DDoS attacks, and integration with AWS WAF for web application firewall protection.
4. **Customization**: customize content delivery through features like custom SSL certificates, custom domain names, and support for HTTP/2 and HTTP/3 protocols.
5. **Integration**: seamlessly integrates with other AWS services, such as S3, EC2, Elastic Load Balancing, and Lambda. It can also be used in conjunction with AWS Elemental Media Services for video streaming.
6. **Real-time Analytics**: provides detailed metrics and logs that give insights into your content delivery performance, including viewer demographics, traffic patterns, and cache utilization.

CloudFront is commonly used to accelerate the delivery of static and dynamic content, including websites, APIs, streaming media, and large file downloads. It's **particularly useful for applications with a global user base**, as it helps to ensure a consistent and high-quality user experience across different geographical regions.

In an AWS architecture, CloudFront is often used in conjunction with other services like S3, EC2, and API Gateway to build scalable and performant web applications and APIs. It can sit in front of these services to cache and deliver content more efficiently to end users, thereby improving overall application performance and reducing latency.

Amazon CloudFront can be used in front of an Application Load Balancer.

## Global Accelerator
- User => Anycast IP => Edge locations => ALB => application
- A networking service to intelligently **route non-cacheable TCP and UDP traffic to the nearest healthy endpoint** so as to improve the availability and performance of applications. 
- It optimizes the delivery of dynamic content, such as gaming, IoT, and real-time communications applications, by leveraging the AWS global network infrastructure.

Improve global application availability and performance by directing traffic to the optimal endpoint based on AWS's global network infrastructure. This can potentially provide better performance compared to routing traffic solely through CloudFront.
![![all-in-one/Internet/3 - Cloud Computing/Understanding AWS/#^Table2]]
**Global Accelerator focuses on optimizing the delivery of non-cacheable TCP and UDP traffic to applications, while CloudFront accelerates the delivery of web content by caching it at edge locations.**

## Disaster Recovery
AWS provides various services and features to support disaster recovery strategies.
- RPO: Recovery Point Objective
- RTO: Recovery Time Objective

## Migration
AWS offers services and tools to migrate workloads to the cloud.