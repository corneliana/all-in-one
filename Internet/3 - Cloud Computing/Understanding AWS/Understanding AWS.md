## What is AWS?
AWS: Amazon Web Services, a cloud computing platform. 

**Cloud computing** is a new layer of abstraction over physical computing resources, which transforms them into **scalable, on-demand services** that can be used without managing the underlying hardware directly.
- **Microservices:** an architectural approach where a software application is composed of **small, independently deployable services** that are organized around specific business capabilities and communicate with each other through well-defined APIs.
- **Serverless:** no need to manage/provision servers anymore, just deploy code and functions.

In a sense, the whole idea of AWS is like customizing a remote computer and its network.
To build it, we will need:

**[[1 - Hard Drive]]**
- **EBS (Elastic Block Store)**: Provides block storage volumes for EC2 instances.
- **EC2 Instance Store**: Provides temporary block-level storage for EC2 instances.

**[[2 - Compute Capacity]]
- **EC2 (Elastic Compute Cloud)**: Provides scalable virtual servers in the cloud.
- **Lambda**: Serverless compute service for running code without provisioning or managing servers.

**[[3 - Containers and Orchestration]]**
- **ECS (Elastic Container Service)**: Manages containers using Docker.
- **Fargate**: Serverless compute engine for containers.
- **ECR (Elastic Container Registry)**: Docker container registry.
- **EKS (Elastic Kubernetes Service)**: Managed Kubernetes service.
- **CloudFormation**: Infrastructure as Code (IaC) service for provisioning AWS resources.

**[[4 - Data Storage]]**
- **EFS (Elastic File System)**: Fully managed file system for EC2 instances.
- **S3 (Simple Storage Service)**: Object storage service.
- **Aurora**: MySQL and PostgreSQL-compatible relational database.
- **RDS (Relational Database Service)**: Managed relational database service.
- **DynamoDB**: Fully managed NoSQL database.
- **ElastiCache**: Managed in-memory data store.
- **Storage Extras**: Not sure what you meant here.


1. **Performance and Availability**:
    - **ELB (Elastic Load Balancing)**: Distributes incoming application traffic across multiple targets.
    - **ASG (Auto Scaling Group)**: Automatically adjusts the number of EC2 instances in response to demand.
    - **CloudFront**: Content Delivery Network (CDN).
    - **Global Accelerator**: Improve global application availability and performance.
    - **Disaster Recovery**: AWS provides various services and features to support disaster recovery strategies.
    - **Migration**: AWS offers services and tools to migrate workloads to the cloud.
2. **Service Communication**:
    - **SQS (Simple Queue Service)**: Fully managed message queuing service.
    - **SNS (Simple Notification Service)**: Fully managed pub/sub messaging service.
    - **Kinesis**: Collect, process, and analyze real-time, streaming data.
    - **Active MQ**: Managed message broker service.
3. **Networking**:
    - **API Gateway**: Fully managed service for creating, publishing, maintaining, monitoring, and securing APIs.
    - **Route 53**: Domain Name System (DNS) web service.
    - **VPC (Virtual Private Cloud)**: Virtual network in the cloud.
4. **Security and Encryption**:
    - **IAM (Identity and Access Management)**: Secure control access to AWS services and resources.
    - **Security groups**: Act as a virtual firewall for your EC2 instances.
    - **KMS (Key Management Service)**: Manage encryption keys.
    - **SSM Parameter Store**: Securely store parameters and secrets.
    - **Shield**: Managed Distributed Denial of Service (DDoS) protection service.
    - **WAF (Web Application Firewall)**: Protect web applications from common web exploits.
5. **Deployment**:
    - **Elastic Beanstalk**: Deploy and manage applications in the cloud without worrying about the infrastructure.
    - **AWS App Runner**: Fully managed service to deploy containerized web applications and APIs.
6. **Monitoring**:
    - **CloudWatch**: Monitoring and observability service.
    - **CloudTrail**: Record AWS API calls for your account.
    - **Config**: Assess, audit, and evaluate the configurations of your AWS resources.
7. **Data Analytics**:
    - **Athena**: Interactive query service for S3 data.
    - **Redshift**: Fully managed data warehouse.
    - **OpenSearch**: Managed Elasticsearch service.
    - **EMR (Elastic MapReduce)**: Managed Hadoop framework.
    - **QuickSight**: Business intelligence service.
    - **Glue**: ETL (Extract, Transform, Load) service.
    - **Lake Formation**: Data lake creation and management service.
    - **Kinesis Data Analytics**: Analyze streaming data.
    - **MSK (Managed Streaming for Kafka)**: Fully managed Apache Kafka service.
    - **Big Data Ingestion Pipeline**: Not sure what specific service you are referring to here.
8. **Other Services**:
    - **SES (Simple Email Service)**: Email sending and receiving service.
    - **Pinpoint**: Targeted user engagement service.
    - **SSM Session Manager**: Provides interactive shell access to instances in your VPC.
    - **Cost Explorer**: Analyze AWS costs and usage.
    - **Batch**: Run batch computing workloads.
    - **AppFlow**: Securely transfer data between AWS services and SaaS applications.
    - **Amplify**: Platform for building scalable mobile and web applications.

