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

## CloudFront: AWS's Content Delivery Network (CDN)
Amazon CloudFront is a content delivery network (CDN) service that accelerates the delivery of web content (static and dynamic) to end users by caching it at edge locations worldwide. It improves the performance of websites, APIs, and streaming media by serving cached content from the nearest edge location, reducing latency and offloading origin servers.

Edge Locations
Edge Locations are strategically distributed around the world, often located in or near major cities and Internet exchange points.

**Edge Locations are optimized for fast content delivery. Why?**
- They leverage Amazon's highly redundant and low-latency network infrastructure, allowing for efficient data transfer. 
- Edge Locations cache frequently accessed content, further reducing latency for subsequent requests.

Lambda@Edge is a feature of CloudFront that lets you run code closer to users, which improves performance and reduces latency.

Amazon CloudFront is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency, high transfer speeds. Amazon CloudFront can be used in front of an Application Load Balancer.

Origins
The origin servers or locations where CloudFront retrieves the content to serve to end users, which can be various types of **servers or storage services**, such as S3 buckets, EC2 instances, Elastic Load Balancers, or custom HTTP servers.

CloudFront uses origins to cache and distribute content globally across its network of edge locations. When a user requests content, CloudFront checks its cache to see if the content is already stored at an edge location near the user. If the content is not in the cache or if it has expired, CloudFront retrieves the content from the origin server specified for that particular distribution.

Origins play a crucial role in determining where CloudFront fetches content from and how it delivers that content to users efficiently. By configuring origins appropriately, you can optimize performance, reduce latency, and ensure high availability for your distributed content.

## Global Accelerator
-AWS Global Accelerator is a networking service designed to improve the availability and performance of applications by intelligently routing non-cacheable TCP and UDP traffic to the nearest healthy endpoint. It optimizes the delivery of dynamic content, such as gaming, IoT, and real-time communications applications, by leveraging the AWS global network infrastructure.

Improve global application availability and performance by directing traffic to the optimal endpoint based on AWS's global network infrastructure. This can potentially provide better performance compared to routing traffic solely through CloudFront.

![![all-in-one/Internet/3 - Cloud Computing/Understanding AWS/#^Table2]]Global Accelerator focuses on optimizing the delivery of non-cacheable TCP and UDP traffic to applications, while CloudFront accelerates the delivery of web content by caching it at edge locations.

## Disaster Recovery
AWS provides various services and features to support disaster recovery strategies.
- RPO: Recovery Point Objective
- RTO: Recovery Time Objective

## Migration
AWS offers services and tools to migrate workloads to the cloud.