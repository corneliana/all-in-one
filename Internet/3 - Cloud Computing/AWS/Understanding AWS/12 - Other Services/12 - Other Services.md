[[12.1 - SES]]
[[12.2 - OpenSearch]]
[[12.3 - Athena]]
[[12.4 - MSK]]
[[12.5 - ACM]]
[[12.6 - Macie]]
[[12.7 - AppConfig]]
[[12.8 - CloudWatch Evidently]]

- **SSM Session Manager**: Provides interactive shell access to instances in your VPC.
	- Allows you to start a secure shell on your EC2 and on-premises servers
	- Other services
		- Run Command
		- Patch Manager: Automates the process of patching managed instances
		- Maintenance Windows: Defines a schedule for when to perform actions on your instances
		- Automation: Simplifies common maintenance and deployment tasks of EC2 instances and other AWS resources
- **Batch**: Run batch computing workloads.
	- Dynamically launch EC2 instances or Spot Instances
	- Batch jobs are defined as Docker images and run on ECS
	- v.s. Lambda
		- Lambda:  
			- Time limit
			- Limited runtimes  
			- Limited temporary disk space • Serverless
		- Batch:
			- No time limit
			- Any runtime as long as it’s packaged as a Docker image
			- Rely on EBS / instance store for disk space
			- Relies on EC2 (can be managed by AWS)
- **AppFlow**: Securely transfer data between AWS services and SaaS applications.
- **Amplify**: Platform for building scalable mobile and web applications.