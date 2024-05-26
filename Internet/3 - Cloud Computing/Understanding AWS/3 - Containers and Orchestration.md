- **Docker
	- Docker is software development platform to deploy apps. Apps are packaged in containers that can be run on any OS. Docker images are stored in Docker repos.
	- v.s. Virtual Machines
		- Resources are shared with the host. Many containers on one server.
	- Dockerfile => Docker Repository => use

- **Docker Containers Management on AWS
	- **ECS Elastic Container Service: auto deployment, scaling and management of containerized applications
		- Mechanism
			- Cluster management: a grouping of container instances (EC2 instances or Fargate tasks) that are managed together as a single unit
			- Resource allocation
			- Task Scheduling
			- Scaling and availability: AWS Application Auto Scaling (task level, not instance level)
		- launch type
			- EC2 launch type: Launch Docker containers on AWS = **Launch ECS Tasks on ECS clusters**. Each EC2 instance must run the ECS agent to register in the ECS cluster.
				- Scaling: ASG or ECS Cluster Capacity Probider
			- Fargate launch type: run containers on AWS without managing any servers.
		- IAM roles 
			- EC2 instance profile
			- ECS task role: IAM Role used by the ECS task itself. Use when container wants to call other AWS services like S3, SQS, etc.
		- LB integrations: ALB, NLB
		- Data Volumes(EFS): Fargate + EFS = Serverless
			- EFS volume can be shared between different EC2 instances and different ECS Tasks. It can be used as a persistent multi-AZ shared storage for containers.
	- Fargate: A serverless compute engine for containers, running containers without managing servers or clusters.
	- ECR Elastic Container Registry: Store and manage Docker images on AWS
		- A fully managed container registry that makes it easy to store, manage, share, and deploy container images. ECR is fully integrated with ECS, allowing easy retrieval of container images from ECR while managing and running containers using ECS.
	- **EKS Elastic Kubernetes Service**: An alternative to ECS and open-source. A managed container orchestration service for running Kubernetes on AWS. EKS nodes => EKS pods = ECS tasks.
		- Node types
			- Managed Node Groups
			- Self-Managed Nodes
			- Fargate: no nodes
		- Data volumes
	
- **ECS (Elastic Container Service)**: Manages containers using Docker.
- **Fargate**: Serverless compute engine for containers.

- **ECR (Elastic Container Registry)**: A Docker container registry for storing, managing, and deploying container images, similar to a photo album for your container snapshots.

- **EKS (Elastic Kubernetes Service)**: Managed Kubernetes service. Offers managed Kubernetes service for orchestrating containerized applications, acting as the event organizer for managing complex container setups.

- **CloudFormation**: Infrastructure as Code (IaC) service for provisioning AWS resources.