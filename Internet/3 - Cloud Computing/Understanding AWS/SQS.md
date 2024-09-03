**Simple Queue Service => queue model

**A fully managed message** queuing service. Producer sends messages to SQS queue, and Consumers poll messages from the queue.
Amazon SQS allows you to retain messages for days and process them later.

- **Mechanism**
	- **Produce and send**: The message will be sent again if not processed in time.
	- **Consume, process and delete**: consumers can be run on EC2/servers/lambda... Consumers can be scaled to handle higher throughput.
		- ASG: to handle big load
			- requests => ASG(enqueue) => SQS => ASG(dequeue) => DB
		- WaitTimeSeconds to start Long Polling: consumers wait for messages to arrive if there is none in the queue.
		- Message Visibility Timeout: Message that being polled will be invisible to other consumers for a period of time. If not deleted, the same message will back to the queue after being timeout is elapsed.
		- ChangeMessageVisibility API to get more time for processing.
	- **Ordering of messages**: FIFO
		- The order in which messages are sent and received are strictly preserved and a message is delivered once and remains available until a consumer process and deletes it. 
		- Duplicated messages are not introduced into the queue.
	
- **Use cases
	- Decouple applications
	- A buffer to DB writes

- **Security
	- Encryption
		- In-flight: HTTPS
		- At-rest: KMS
		- Client-side
	- Access Controls: IAM
	- Access Policies: similar to S3

- **Dead Letter Queue