## VPC: Virtual Private Cloud
Virtual network in the cloud.
A gateway VPC endpoint privately connect VPC to supported AWS services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection.
![[VPC-components-diagram.png]]

AWS Network Firewall is a managed firewall service that provides stateful inspection and filtering of network traffic at the perimeter of VPCs. It allows you to define rules based on IP addresses, ports, and protocols to control traffic flow and enforce security policies. Network Firewall is designed for filtering traffic at the network level within a VPC, making it suitable for implementing traffic inspection and filtering.

Traffic Mirroring allows to copy network traffic from Elastic Network Interfaces (ENIs) of Amazon EC2 instances within a VPC to a destination for analysis. This could potentially be used for traffic inspection and filtering by forwarding mirrored traffic to an inspection server or security appliance, similar to the on-premises inspection server mentioned in the scenario.


## API Gateway
API Gateway is a fully managed service that makes it easy to create, publish, maintain, monitor, and secure APIs at any scale

API Gateway is primarily designed for creating APIs to expose backend services or data to external clients, rather than for internal VPC-to-VPC communication.

An Edge-Optimized API Gateway is best for geographically distributed clients. API requests are routed to the nearest CloudFront Edge Location which improves latency. The API Gateway still lives in one AWS Region.

## Route 53: Domain Name Service (DNS)
