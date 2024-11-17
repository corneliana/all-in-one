## Metrics
- Provides metrics for every service in AWS
- Metric is a variable to monitor (CPUUtilization, NetworkIn…)
- Metrics belong to namespaces
- Dimension is an attribute of a metric (instance id, environment, etc…).
- Up to 30 dimensions per metric
- Metrics have timestamps
- Can create CloudWatch dashboards of metrics

- High-Resolution Custom Metrics can have a minimum resolution of 1 second.
- If you set an alarm on a high-resolution metric, specify a high-resolution alarm with a period of 10 or 30 seconds, or set a regular alarm with a period of any multiple of 60 seconds.

### Custom Metrics
- API call: PutMetricData
- Accepts metric data points two weeks in the past and two hours in the future

## Logs
- Log groups: representing an application
- Log stream: instances within application / log files / containers
- Log expiration policies. By default, the logs never expire.
- Logs are encrypted by default
- Can setup KMS-based encryption with your own keys
- EC2 Detailed Monitoring: EC2 instance's metrics are available in 1-minute periods.

### Insights
- Search logs and get logs visualization by log groups and timeframe

### Sources
- SDK, CloudWatch Logs Agent, CloudWatch Unified Agent
- Elastic Beanstalk: collection of logs from application
- ECS: collection from containers
- AWS Lambda: collection from function logs
- VPC Flow Logs: VPC specific logs
- API Gateway
- CloudTrail based on filter
- Route53: Log DNS queries

## Targets
- Amazon S3 (exports)
	- Log data can take up to 12 hours to become available for export
	- The API call is CreateExportTask
	- Not near-real time or real-time… use Logs Subscriptions instead
- Kinesis Data Streams
- Kinesis Data Firehose
- AWS Lambda
- OpenSearch

### Logs Subscriptions
- Get a real-time log events from CloudWatch Logs for processing and analysis
- Send to Kinesis Data Streams, Kinesis Data Firehose, or Lambda
- Subscription Filter – filter which logs are events delivered to your destination
![[CloudWatch logs subscription.png]]

### Logs Aggregation Multi-Account & Multi Region

![[CloudWatch Logs Aggregation.png]]

- Cross-Account Subscription
![[Cross-Account Subscription.png]]

### Logs Agent & Unified Agent
- By default, no logs from the EC2 machine will go to CloudWatch
- So will use CloudWatch Agent to push logs from EC2 to CloudWatch
- Unified Agent gets more metrics

### Logs Metric Filter
- Filters do not retroactively filter data. Filters only publish the metric for events that happens after the filter is created.

### Alarms
- Triggered for any metric
- Various options: max, min, sampling, %
- Targets:
	- Stop, Terminate, Reboot, or Recover an EC2 Instance
	- Trigger Auto Scaling Action
	- Send notification to SNS (from which you can do pretty much anything)
- Composite Alarms: combined alarms, AND and OR conditions
- Alarms can be created based on CloudWatch Logs Metrics Filter

### Synthetics Canary
?
