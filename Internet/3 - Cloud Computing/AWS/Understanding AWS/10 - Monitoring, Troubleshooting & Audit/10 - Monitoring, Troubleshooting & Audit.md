[[10.1 - CloudWatch]]
[[10.2 - EventBridge]]
[[10.3 - X-Ray]]
[[10.4 - CloudTrail]]

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
- CloudWatch:
	- CloudWatch Metrics over time for monitoring
	- CloudWatch Logs for storing application log
	- CloudWatch Alarms to send notifications in case of unexpected metrics
- X-Ray:
	- Automated Trace Analysis & Central Service Map Visualization
	- Latency, Errors and Fault analysis
	- Request tracking across distributed systems
