AWS: Amazon Web Services, a cloud computing platform. Cloud computing is a new layer of abstraction over physical computing resources, which transforms them into scalable, on-demand services that can be used without managing the underlying hardware directly.

The platform is used in a specific region => **Availability Zones (AZ)**
- global services: IAM, Route 53(DNS Service), CloudFront(Content Delivery Network), WAF(Web Application Firewall)
- regional services: EC2(IaaS), Elastic Beanstalk(PaaS), Lambda(FaaS), Rekognition(SaaS)

In a sense, the whole idea of AWS is like assembling a remote computer piece by piece according to my needs.

## 1. Choose the right type of computers (the right service)
### EC2: Elastic Compute Cloud = Infrastructure as a Service
An Amazon EC2 instance is like a virtual computer running in the cloud, similar to CPU and RAM of a computer. Choose its types and sizes based on computational power and memory requirements to determine the performance capabilities.
### Lambda
For tasks that don't need a dedicated computer running all the time. It's like having robots wake up, perform a task quickly, and go back to sleep without worrying about the electricity bill.

## 2. Setting up the hard drive (storage solutions)
### EBS: Elastic Block Store
Like a physical computer needs a hard drive to **store its operating system, applications, and data**, an EC2 instance requires a virtual hard drive. This is where EBS volumes come into play.
**Volume v.s. Snapshot**
- Volume is a durable, block-level **storage device** that is attached to a single EC2 instance to ensure persistent, flexible, and high-performance storage.
- Snapshot is a point-in-time copy of a Volume, which are stored in Amazon S3 and provide a historical record of a volume's state, providing a robust backup and disaster recovery.
### S3: Simple Storage Service
Think of S3 as an infinitely large external hard drive or cloud storage service to store and retrieve any amount of data, at any time, from anywhere on the web. It's great for data backup, archiving, and serving web content.

Note: Storage is where data is kept long-term, and memory is where data is kept short-term for quick access by the computer's processor.

## 3. Organize and Access files (db and content delivery)
### EFS: Elastic File System
A scalable, elastic, cloud-native NFS file system for Linux-based workloads for use with AWS Cloud services and on-premises resources. It is highly available and durable, allowing multiple EC2 instances to access the file system at the same time, much like sharing files across different computers on the same network.
### RDS: Relational Database Service
A managed database service, like having a specialized filing cabinet for the structured data that takes care of organizing, retrieving, and storing efficiently.
### DynamoDB
A NoSQL database service for unstructured data, like a magical notebook that instantly stores and retrieves notes no matter how many you have.
### CloudFront
A content delivery network that ensures website's content is delivered quickly to viewers, like having express delivery trucks ready to send your data wherever it needs to go.

## 4. Keep the remote computer safe (security and identity services)
### IAM: Identity and Access Management
A security system that ensures only the people you've given keys to can enter the house or specific rooms (services) you've designated.
### Security Groups
Like setting up a security system for your home, deciding who can knock on the door (access your instances) and who can't.

## 5. Connecting Computer to the Internet (networking)

### VPC: Virtual Private Cloud
Like setting up private Wi-Fi network for the remote computer by defining a virtual network in AWS cloud logically isolated from other virtual networks. It gives control over the network environment, including IP address range, subnets, routing tables, and network gateways.
### Route 53
A service that translates human-friendly website names (like [www.example.com](http://www.example.com/)) into IP addresses that computers use to identify each other.

## 6. Management and Monitoring
### CloudWatch
A monitoring tool that keeps an eye on the cloud resources and applications, like having a smart home system that alerts if anything unusual happens.
### Auto Scaling
Automatically adjusts the number of EC2 instances to match demand, like having a house that can magically expand or shrink based on guests' amount.

## 7. Building and Deploying Application (Developer Tools)

### CodeDeploy, CodeBuild, CodePipeline
These services automate code deployment, building, and integration process, akin to having a team of robots that continuously improve and fix your house without you lifting a finger.

## ASG

## Aurora

## Elastic Cache

## Quiz

You are running a high-performance database that requires an IOPS of 310,000 for its underlying storage. What do you recommend?

Use an EC2 instance store

You can run a database on an EC2 instance that uses an Instance Store, but you'll have a problem that the data will be lost if the EC2 instance is stopped (it can be restarted without problems). One solution is to set up a replication mechanism on another EC2 instance with an Instance Store to have a standby copy. Another solution is to set up backup mechanisms for your data. It's all up to you how you want to set up your architecture to validate your requirements. In this use case, it's around IOPS, so we have to choose an EC2 Instance Store.







