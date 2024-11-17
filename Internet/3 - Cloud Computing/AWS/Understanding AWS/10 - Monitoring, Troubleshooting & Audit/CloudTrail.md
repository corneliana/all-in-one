
## Diagram
![[CloudTrail Diagram.png]]

## Events
- Management events
- Data Events
- CloudTrail Insights Events

## Insights
- So many events => insights to detect unusual activity, and continuously analyzes write events to detect unusual patterns
	- Anomalies appear in the CloudTrail Console
	- Event is sent to S3
	- An EventBridge event is generated

## Events Retention
- 90 days
- Longer than 90 days, log them to S3 and use Athena

## Integrated with EventBridge
- Intercept API Calls
![[Intercept API Calls.png]]
- EventBridge + CloudTrail
![[EventBridge + CloudTrail.png]]