## VPC: Virtual Private Cloud
A private virtual network to control over the network environment, including IP address range, subnets, routing tables, and network gateways.

![[VPC-components-diagram.png]]

Public & private IP 

AWS Network Firewall is a managed firewall service that provides stateful inspection and filtering of network traffic at the perimeter of VPCs. It allows you to define rules based on IP addresses, ports, and protocols to control traffic flow and enforce security policies. Network Firewall is designed for filtering traffic at the network level within a VPC, making it suitable for implementing traffic inspection and filtering.

Traffic Mirroring allows to copy network traffic from Elastic Network Interfaces (ENIs) of Amazon EC2 instances within a VPC to a destination for analysis. This could potentially be used for traffic inspection and filtering by forwarding mirrored traffic to an inspection server or security appliance, similar to the on-premises inspection server mentioned in the scenario.

### VPC Summary
- Default VPC
	- All new AWS accounts have a default VPC
	- New EC2 instances are launched into the default VPC if no subnet is specified
	- Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses
	- We also get a public and a private IPv4 DNS names
	
- CIDR – IP Range
	- Classless Inter-Domain Routing – a method for allocating IP addresses
	- Base IP: Represents an IP contained in the range (XX.XX.XX.XX). Example:10.0.0.0,192.168.0.0,...
	- Subnet Mask: Defines how many bits can change in the IP. It basically allows part of the underlying IP to get additional next values from the base IP. Example:/0,/24,/32
		- Can take two forms: 
			- /8ó255.0.0.0
			- /16ó255.255.0.0  
			- /24ó255.255.255.0  
			- /32ó255.255.255.255
	
- VPC – Virtual Private Cloud => we define a list of IPv4 & IPv6 CIDR
	- You can have multiple VPCs in an AWS region (max. 5 per region – soft limit)
	- Max. CIDR per VPC is 5, for each CIDR: • Min. size is /28 (16 IP addresses)  
		- Max. size is /16 (65536 IP addresses)
	- Because VPC is private, only the Private IPv4 ranges are allowed:
		- 10.0.0.0 – 10.255.255.255 (10.0.0.0/8)  
		- 172.16.0.0 – 172.31.255.255 (172.16.0.0/12)  
		- 192.168.0.0 – 192.168.255.255 (192.168.0.0/16)
	- Your VPC CIDR should NOT overlap with your other networks (e.g., corporate)
	
- Subnets – tied to an AZ, we define a CIDR
	- AWS reserves 5 IP addresses (first 4 & last 1) in each subnet
	- These 5 IP addresses are not available for use and can’t be assigned to an EC2 instance
	- if you need 29 IP addresses for EC2 instances:  
		- You can’t choose a subnet of size /27 (32 IP addresses, 32 – 5 = 27 < 29)  
		- You need to choose a subnet of size /26 (64 IP addresses, 64 – 5 = 59 > 29)

- **Gateway v.s. Endpoints**
	- While both facilitate connectivity in AWS networking, **gateways typically provide connectivity between your VPC and external networks**, while **endpoints provide access points for interacting with specific AWS services within your VPC.**

- Internet Gateway(IGW) – at the VPC level, provide IPv4 & IPv6 Internet Access
	- Allows resources (e.g., EC2 instances) in a VPC connect to the Internet
	- It scales horizontally and is highly available and **redundant**  
	- Must be created separately from a VPC  
	- One VPC can only be attached to one IGW and vice versa
	- Internet Gateways on their own do not allow Internet access...
	- Route tables must also be edited!
	
- Route Tables – must be edited to add routes from subnets to the IGW,VPC Peering Connections,VPC Endpoints, ...

- Bastion Host – public EC2 instance to SSH into, that has SSH connectivity to EC2 instances in private subnets
	- Use a Bastion Host to SSH into our private EC2 instances
	- The bastion is in the public subnet which is then connected to all other private subnets
	- Bastion Host security group must allow inbound from the internet on port 22 from restricted CIDR, for example the public CIDR of your corporation
	- Security Group of the EC2 Instances must allow the Security Group of the Bastion Host, or the private IP of the Bastion host
	
- NAT Instances - give Internet access to EC2 instances in private subnets. Old, must be setup in a public subnet, disable Source / Destination check flag for it to work and also edit the Security Group Rules
	- Allows EC2 instances in private subnets to connect to the Internet
	- Must be launched in a public subnet
	- Must disable EC2 setting: Source / destination Check
	- Must have Elastic IP attached to it
	- RouteTables must be configured to route traffic from private subnets to the NAT Instance
	
- NAT Gateway – managed by AWS, provides scalable Internet access to private EC2 instances, IPv4 only
	- AWS-managed NAT, higher bandwidth, high availability, no administration
	- NATGW is created in a specific Availability Zone, uses an Elastic IP
	- NAT Gateway is resilient within a single Availability Zone
	- Must create multiple NAT Gateways in multiple AZs for fault-tolerance
	- There is no cross-AZ failover needed because if an AZ goes down it doesn't need NAT
	![[Pasted image 20240517000631.png]]

- NACL – **stateless**, subnet rules for inbound and outbound, don’t forget Ephemeral Ports
- Security Groups – **stateful**, operate at the EC2 instance level
- Security Groups & NACLs
 ![[Pasted image 20240517000800.png]]

- VPC Peering – connect two VPCs with non overlapping CIDR, non-transitive

- VPC Endpoints – **provide private access to AWS Services (S3, DynamoDB, CloudFormation, SSM) within a VPC**
	- **S3 and DynamoDB have a VPC Gateway Endpoint (remember it), all the other ones have an Interface endpoint (powered by Private Link - means a private IP).**
	- Gateway Endpoint v.s. Interface Endpoint
		- Both facilitate private connectivity to AWS services within a VPC, they have different implementations
		- **Gateway endpoints** are specific to certain AWS services like **S3 and DynamoDB**. They are implemented using VPC route tables and provide a direct, targeted route for traffic destined to these services. A gateway VPC endpoint privately connect VPC to supported AWS services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect.
		- **Interface endpoints** are powered by AWS PrivateLink and support a broader range of AWS services. They use Elastic Network Interfaces (ENIs) within your VPC to establish private connections to the service endpoints. They offer more flexibility and are integrated with AWS PrivateLink's service discovery and endpoint policies.
	
- VPC Flow Logs – can be setup at the VPC / Subnet / ENI Level, for ACCEPT and REJECT traffic, helps identifying attacks, analyze using Athena or CloudWatch Logs Insights
	- VPC Flow Logs is a VPC feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC.

- AWS VPN CloudHub – hub-and-spoke VPN model to connect your sites
	- It allows you to securely communicate with multiple sites using AWS VPN. It operates on a simple hub-and-spoke model that you can use with or without a VPC.
- Site-to-Site VPN – A Site-to-Site VPN securely connects your on-premises network to your AWS infrastructure over the internet using encryption tunnels.
	- **Site-to-Site VPN** is ideal for scenarios where **cost concerns are paramount, and the flexibility and quick setup are required**. It's also useful when **the variability in performance of internet-based connections is acceptable**, or when **encrypted data transmission is needed without the higher cost of a dedicated line**.

- Direct Connect – setup a Virtual Private Gateway on VPC, and establish a direct private / dedicated connection to an AWS Direct Connect Location.
	- **Direct Connect** is best utilized when there is a need for **high throughput, consistent performance, and perhaps lower data transfer costs at higher volumes**. It's also suitable when regulatory compliance dictates a direct connection.
- Direct Connect Gateway – setup a Direct Connect to many VPCs in different AWS regions

- AWS PrivateLink / VPC Endpoint Services:  
	- Connect services privately from your service VPC to customers VPC
	- Doesn’t need VPC Peering, public Internet, NAT Gateway, Route Tables • Must be used with Network Load Balancer & ENI
	
- ClassicLink – connect EC2-Classic EC2 instances privately to your VPC

- Transit Gateway – transitive peering connections for VPC, VPN & DX

- Traffic Mirroring – copy network traffic from ENIs for further analysis
	- Use cases include content inspection, threat monitoring, and troubleshooting.
	
- Egress-only Internet Gateway – like a NAT Gateway, but for IPv6
	- Allows only outbound traffic
![[NAT-gateway-vs-Egress-only-Internet-gateway.png]]


### Networking Costs in AWS per GB

### Security
Referencing by security groups in rules is an extremely powerful rule and many questions at the exam rely on it. Make sure you fully master the concepts behind it!

An "inspection VPC" refers to a Virtual Private Cloud (VPC) configuration within the context of network security and traffic inspection architectures. In this setup, the inspection VPC is specifically designed to analyze and monitor network traffic passing through it for security or compliance purposes.

## API Gateway

- A **fully managed service** to create, publish, maintain, monitor, and secure APIs at any scale. 
- A front door for applications to access data, business logic, or functionality hosted on backend services, such as Lambda functions, EC2 instances, or HTTP endpoints. 
- provides features like API creation, versioning, deployment, security, monitoring, and **throttling** => throttles requests to your API using the token bucket algorithm, where a token counts for a request. Specifically, API Gateway sets a limit on a steady-state rate and a burst of request submissions against all APIs in your account. In the token bucket algorithm, the burst is the maximum bucket size. 
	- tokens in this model help to smooth out the handling of API requests during both normal and high-traffic conditions, ensuring a more reliable and consistent service experience.

- Edge-Optimized(default): For global clients
	- Requests are routed through the CloudFront Edge locations (improves latency)
	- The API Gateway still lives in only one region
- Regional
	- For clients within the same region
	- Could manually combine with CloudFront (more control over the caching strategies and the distribution)
- Private
	- Can only be accessed from your VPC using an interface VPC endpoint (ENI)
	- Use a resource policy to define access

Key features of API Gateway include:
- **API Creation and Management**: You can define APIs using API Gateway's RESTful interface or WebSocket APIs for real-time communication.
- **Integration with Backend Services**: API Gateway can integrate with various backend services, including Lambda functions, EC2 instances, HTTP endpoints, and other AWS services.
- **Security**: API Gateway supports authentication and authorization mechanisms such as AWS IAM, OAuth 2.0, and custom authorizers to control access to your APIs.
- **Monitoring and Logging**: API Gateway provides detailed monitoring metrics, logging, and tracing capabilities to help you troubleshoot and optimize your APIs.

API Gateway is commonly used for building RESTful APIs, GraphQL APIs, WebSocket APIs, and serverless applications, allowing developers to expose their backend services securely and efficiently to external clients.

## Route 53: Domain Name Service (DNS)
Translate human-readable domain names into IP addresses, allowing users to access resources on the internet using familiar domain names.

Key features of Route 53 include:
- **Domain Registration**: You can register domain names directly through Route 53. Route 53 is DNS service, GoDaddy is Domain Registrar. We purchase the domain from GoDaddy and use Route 53 to manage DNS records.
- **DNS Routing**: Route 53 allows you to route traffic based on various policies to different endpoints, such as Amazon EC2 instances, Elastic Load Balancers, Amazon S3 buckets, or even external resources.
- **Health Checks**: Route 53 can monitor the health of your resources and route traffic away from unhealthy endpoints.
- **Traffic Flow**: Route 53 Traffic Flow allows you to visually manage how traffic is routed to your resources, making it easier to implement complex routing configurations.

Route 53 is commonly used for hosting public-facing websites, managing internal DNS for private resources, and implementing global load balancing and failover solutions.

#### Routing policies: define a DNS record (key-value pair with attributes)
- **DNS record types**
	-  A
	- AAAA
	- CNAME
	- NS
	- Alias v.s. CNAME
		- CNAME: points a hostname to any other hostnames
		- Alias: points a hostname to an AWS resource. Free of charge. Native health check.
- **Based on**
	- Simple
	- Weighted: redirect part of the traffic based on weight (e.g. percentage). It's a common use case to send part of traffic to a new version of application.
	- Latency: evaluate the latency between users and AWS Regions, and help them get a DNS response that will minimize their latency (e.g. response time)
	- Failover: instance 1 not healthy, route to instance 2
	- Geolocation:
	- Geoproximity: bias
	- IP
	- Multi-value
- **TTL**
	Each DNS record has a TTL (Time To Live) which orders clients for how long to cache these values and not overload the DNS Resolver with DNS requests. The TTL value should be set to strike a balance between how long the value should be cached vs. how many requests should go to the DNS Resolver.
	- High TTL: Less traffic on Route 53; possibly outdated records
	- Low TTL: More traffic on Route 53; records are outdated for less time; easy to change records so users will experience less downtime.

#### Health checks
- Monitor an endpoint
- Calculated Health checks
- Private Hosted Zones
