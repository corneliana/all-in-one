[[CloudWatch]]
[[EventBridge]]
[[X-Ray]]
[[CloudTrail]]

## Monitoring in AWS

- **Why Monitoring is Important?**
	- We know how to deploy applications
		- Safely
		- Automatically
		- Using Infrastructure as Code
		- Leveraging the best AWS components!
	- Our applications are deployed, and our users don’t care how we did it…
	- Our users only care that the application is working!
	- Application latency: will it increase over time?
	- Application outages: customer experience should not be degraded
	- Users contacting the IT department or complaining is not a good outcome
	- Troubleshooting and remediation

- Internal monitoring:
	- Can we prevent issues before they happen?
	- Performance and Cost
	- Trends (scaling patterns)
	- Learning and Improvement

- **AWS CloudWatch:**
	- Metrics: Collect and track key metrics
	- Logs: Collect, monitor, analyze and store log files
	- Events: Send notifications when certain events happen in your AWS
	- Alarms: React in real-time to metrics / events
	- EC2 Detailed Monitoring
		- EC2 instance metrics have metrics “every 5 minutes”
		- With detailed monitoring (for a cost), you get data “every 1 minute”
		- Use detailed monitoring if you want to scale faster for your ASG!
		- The AWS Free Tier allows us to have 10 detailed monitoring metrics
		- Note: EC2 Memory usage is by default not pushed (must be pushed from inside the instance as a custom metric)
- **AWS X-Ray:**
	- Troubleshooting application performance and errors
	- Distributed tracing of Microservices
- **AWS CloudTrail:**
	- Internal monitoring of API calls being made
	- Audit changes to AWS Resources by users

**CloudTrail vs CloudWatch vs X-Ray**
- CloudTrail:
	- Audit API calls made by users / services / AWS console
	- Useful to detect unauthorized calls or root cause of changes
	- CloudTrail allows you to log, continuously monitor, and retain account activity related to actions across AWS infrastructure. It provides the event history of AWS account activity, audit API calls made through the AWS Management Console, SDKs, CLI. So, the EC2 instance termination API call will appear here. You can use CloudTrail to detect unusual activity in your AWS accounts.
- CloudWatch:
	- CloudWatch Metrics over time for monitoring
	- CloudWatch Logs for storing application log
	- CloudWatch Alarms to send notifications in case of unexpected metrics
	- A monitoring service that allows you to monitor applications, respond to system-wide performance changes, optimize resource utilization, and get a unified view of operational health. It is used to monitor applications' performance and metrics.
- X-Ray:
	- Automated Trace Analysis & Central Service Map Visualization
	- Latency, Errors and Fault analysis
	- Request tracking across distributed systems

## CloudWatch v.s. CloudTrail v.s. Config
- **CloudWatch v.s. CloudTrail v.s. Config**
	- CloudWatch  
		- Performance monitoring (metrics, CPU, network, etc...) & dashboards
		- Events & Alerting  
		- Log Aggregation & Analysis
	- CloudTrail  
		- Record **a history of AWS API calls** made within your Account by everyone
			- Use the CloudTrail Console to view the last 90 days of recorded API activity. For events older than 90 days, use Athena to analyze CloudTrail logs stored in S3.
		- Can define trails for specific resources  
		- Global Service
	- Config
		- Record configuration changes
		- Evaluate resources against compliance rules
		- Get timeline of changes and compliance
	
- **For an Elastic Load Balancer**
	- CloudWatch:  
		- Monitoring Incoming connections metric
		- Visualize error codes as % over time  
		- Make a dashboard to get an idea of your load balancer performance
	- Config:  
		- Track security group rules for the Load Balancer  
		- Track configuration changes for the Load Balancer  
		- Ensure an SSL certificate is always assigned to the Load Balancer (compliance)
	- CloudTrail:  
		- Track who made any changes to the Load Balancer with API calls

## Config
- What is Config?
	- **Assess, audit, and evaluate the compliances and configurations of the AWS resources.**
	- Questions that can be solved by AWS Config:  
		- Is there unrestricted SSH access to my security groups?
		- Do my buckets have any public access?  
		- How has my ALB configuration changed over time?
	- You can receive alerts (SNS notifications) for any changes  
	- AWS Config is a per-region service  
	- Can be aggregated across regions and accounts  
	- Possibility of storing the configuration data into S3 (analyzed by Athena)

- Config rules
	- Creation
		- AWS managed rules
		- Customized rules
		- AWS Config Rules does not prevent actions from happening (no deny)
		- Pricing: no free tier, $0.003 per configuration item recorded per region, $0.001 per config rule evaluation per region
	- Remediations of non-compliant resources using SSM/custom Automation docs
		- Tip: can create custom Automation Documents that invokes Lambda function
		- Can set Remediation Retries if resource still non-compliant after auto-remediation
	- Notifications
		- Use EventBridge to trigger notifications when AWS resources are non- compliant
		- Ability to send configuration changes and compliance state notifications to SNS (all events – use SNS Filtering or filter at client-side)