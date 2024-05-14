## VPC: Virtual Private Cloud

A gateway VPC endpoint privately connect VPC to supported AWS services without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection.


## API Gateway
API Gateway is a fully managed service that makes it easy to create, publish, maintain, monitor, and secure APIs at any scale

API Gateway is primarily designed for creating APIs to expose backend services or data to external clients, rather than for internal VPC-to-VPC communication.

An Edge-Optimized API Gateway is best for geographically distributed clients. API requests are routed to the nearest CloudFront Edge Location which improves latency. The API Gateway still lives in one AWS Region.

## Route 53: Domain Name Service (DNS)
