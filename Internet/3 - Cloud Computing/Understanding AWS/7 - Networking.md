## VPC: Virtual Private Cloud
Virtual network in the cloud.
A gateway VPC endpoint privately connect VPC to supported AWS services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection.
![[VPC-components-diagram.png]]

- Default VPC Walkthrough
	- All new AWS accounts have a default VPC
	- New EC2 instances are launched into the default VPC if no subnet is specified
	- Default VPC has Internet connectivity and all EC2 instances inside it have public IPv4 addresses
	- We also get a public and a private IPv4 DNS names
- VPC in AWS - IPv4
	- You can have multiple VPCs in an AWS region (max. 5 per region – soft limit)
	- Max. CIDR per VPC is 5, for each CIDR: • Min. size is /28 (16 IP addresses)  
		- Max. size is /16 (65536 IP addresses)
	- Because VPC is private, only the Private IPv4 ranges are allowed:
		- 10.0.0.0 – 10.255.255.255 (10.0.0.0/8)  
		- 172.16.0.0 – 172.31.255.255 (172.16.0.0/12)  
		- 192.168.0.0 – 192.168.255.255 (192.168.0.0/16)
	- Your VPC CIDR should NOT overlap with your other networks (e.g., corporate)
- Subnet
	- AWS reserves 5 IP addresses (first 4 & last 1) in each subnet
	- These 5 IP addresses are not available for use and can’t be assigned to an EC2 instance
	- if you need 29 IP addresses for EC2 instances:  
		- You can’t choose a subnet of size /27 (32 IP addresses, 32 – 5 = 27 < 29)  
		- You need to choose a subnet of size /26 (64 IP addresses, 64 – 5 = 59 > 29)
- Internet Gateway(IGW)
	- Allows resources (e.g., EC2 instances) in a VPC connect to the Internet
	- It scales horizontally and is highly available and **redundant**  
	- Must be created separately from a VPC  
	- One VPC can only be attached to one IGW and vice versa
	- Internet Gateways on their own do not allow Internet access...
	- Route tables must also be edited!
- Bastion Hosts
	- Use a Bastion Host to SSH into our private EC2 instances
	- The bastion is in the public subnet which is then connected to all other private subnets
	- Bastion Host security group must allow inbound from the internet on port 22 from restricted CIDR, for example the public CIDR of your corporation
	- Security Group of the EC2 Instances must allow the Security Group of the Bastion Host, or the private IP of the Bastion host
- NAT Instance: Network Address Translation
	- Allows EC2 instances in private subnets to connect to the Internet
	- Must be launched in a public subnet
	- Must disable EC2 setting: Source / destination Check
	- Must have Elastic IP attached to it
	- RouteTables must be configured to route traffic from private subnets to the NAT Instance
- NAT Gateway
	- AWS-managed NAT, higher bandwidth, high availability, no administration
	- NATGW is created in a specific Availability Zone, uses an Elastic IP
	- NAT Gateway is resilient within a single Availability Zone
	- Must create multiple NAT Gateways in multiple AZs for fault-tolerance
	- There is no cross-AZ failover needed because if an AZ goes down it doesn't need NAT
	![[Pasted image 20240517000631.png]]

- Security Groups & NACLs
 ![[Pasted image 20240517000800.png]]
- Network Access Control List
- Ephemeral Ports
- NACL with Ephemeral Ports

- CIDR: Classless Inter-Domain Routing - a method of allocating IP addresses
	- Base IP: Represents an IP contained in the range (XX.XX.XX.XX). Example:10.0.0.0,192.168.0.0,...
	- Subnet Mask: Defines how many bits can change in the IP. It basically allows part of the underlying IP to get additional next values from the base IP. Example:/0,/24,/32
		- Can take two forms: 
			- /8ó255.0.0.0
			- /16ó255.255.0.0  
			- /24ó255.255.255.0  
			- /32ó255.255.255.255

Public & private IP 

AWS Network Firewall is a managed firewall service that provides stateful inspection and filtering of network traffic at the perimeter of VPCs. It allows you to define rules based on IP addresses, ports, and protocols to control traffic flow and enforce security policies. Network Firewall is designed for filtering traffic at the network level within a VPC, making it suitable for implementing traffic inspection and filtering.

Traffic Mirroring allows to copy network traffic from Elastic Network Interfaces (ENIs) of Amazon EC2 instances within a VPC to a destination for analysis. This could potentially be used for traffic inspection and filtering by forwarding mirrored traffic to an inspection server or security appliance, similar to the on-premises inspection server mentioned in the scenario.

[[VPC peering]]

VPC Endpoints

VPC Flow Logs

AWS Site-to-Site VPN

AWS VPN CloudHub

VPC - Traffic Mirroring

IPv6

### VPC Summary
- CIDR – IP Range
- VPC – Virtual Private Cloud => we define a list of IPv4 & IPv6 CIDR
- Subnets – tied to an AZ, we define a CIDR
- Internet Gateway – at the VPC level, provide IPv4 & IPv6 Internet Access
- Route Tables – must be edited to add routes from subnets to the IGW,VPC Peering Connections,VPC Endpoints, ...
- Bastion Host – public EC2 instance to SSH into, that has SSH connectivity to EC2 instances in private subnets
- NAT Instances – gives Internet access to EC2 instances in private subnets. Old, must be setup in a public subnet, disable Source / Destination check flag
- NAT Gateway – managed by AWS, provides scalable Internet access to private EC2 instances, IPv4 only
- NACL – stateless, subnet rules for inbound and outbound, don’t forget Ephemeral Ports
- Security Groups – stateful, operate at the EC2 instance level
- VPC Peering – connect two VPCs with non overlapping CIDR, non-transitive
- VPC Endpoints – provide private access to AWS Services (S3, DynamoDB, CloudFormation, SSM) within a VPC
- VPC Flow Logs – can be setup at the VPC / Subnet / ENI Level, for ACCEPT and REJECT traffic, helps identifying attacks, analyze using Athena or CloudWatch Logs Insights
- Site-to-Site VPN – setup a Customer Gateway on DC, a Virtual Private Gateway on VPC, and site-to-site VPN over public Internet
- AWS VPN CloudHub – hub-and-spoke VPN model to connect your sites
- Direct Connect – setup a Virtual Private Gateway on VPC, and establish a direct private connection to an AWS Direct Connect Location
- Direct Connect Gateway – setup a Direct Connect to many VPCs in different AWS regions
- AWS PrivateLink / VPC Endpoint Services:  
	- Connect services privately from your service VPC to customers VPC
	- Doesn’t need VPC Peering, public Internet, NAT Gateway, Route Tables • Must be used with Network Load Balancer & ENI
- ClassicLink – connect EC2-Classic EC2 instances privately to your VPC
- Transit Gateway – transitive peering connections forVPC,VPN & DX
- Traffic Mirroring – copy network traffic from ENIs for further analysis
- Egress-only Internet Gateway – like a NAT Gateway, but for IPv6

## API Gateway
API Gateway is a fully managed service that makes it easy to create, publish, maintain, monitor, and secure APIs at any scale

API Gateway is primarily designed for creating APIs to expose backend services or data to external clients, rather than for internal VPC-to-VPC communication.

An Edge-Optimized API Gateway is best for geographically distributed clients. API requests are routed to the nearest CloudFront Edge Location which improves latency. The API Gateway still lives in one AWS Region.

## Route 53: Domain Name Service (DNS)
