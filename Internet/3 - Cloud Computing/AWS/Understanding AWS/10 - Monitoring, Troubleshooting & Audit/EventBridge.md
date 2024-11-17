Formerly known as CloudWatch events. Now the enhanced version of CloudWatch events.
- Schedule: Cron jobs (scheduled scripts)
	- schedule + trigger script on lambda
- Event Pattern: Event rules to react to a service doing something. AWS services, partner applications, custom.
	- IAM root user sign in event => SNS topic with email notification
- Trigger Lambda functions, send SQS/SNS messages

- EventBridge Rules
![[EventBridge Rules.png]]
- Event buses: receive events from various sources and match them to rules
	- Event buses can be accessed by other AWS accounts using Resource-based Policies
	- You can archive events (all/filter) sent to an event bus (indefinitely or set period)
	- Ability to replay archived events

## Schema Registry
- EventBridge can analyze the events in your bus and infer the schema
- allows to generate code for the application, that will know in advance how data is structured in the event bus
- Schema can be versioned

## Resource-based Policy
- Manage permissions for a specific Event Bus
- Example: allow/deny events from another AWS account or AWS region
- Use case: aggregate all events from AWS organization in a single AWS account or AWS region

## Multi-account Aggregation
![[EventBridge Multi-account Aggregation.png]]