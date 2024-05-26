**Decoupling applications while enable internal communications:** SQS, SNS, Kinesis, Active MQ
- Sync: app to app. Can be overwhelmed in terms of sudden spikes of traffic.
- Async/Event-based: app to queue to app

Amazon Kinesis Data Streams (KDS) is a massively scalable and durable real-time data streaming service. It can continuously capture gigabytes of data per second from hundreds of sources such as website clickstreams, database event streams, financial transactions, social media feeds, IT logs, and location-tracking events.

### SQS: Simple Queue Service => queue model
A **fully managed message** queuing service. Producer sends messages to SQS queue, and Consumers poll messages from the queue.
Amazon SQS allows you to retain messages for days and process them later.
- **Mechanism
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

### SNS: Simple Notification Service => pub/sub model
 The "town crier" for broadcasting messages or alerts, i.e. send messages to many receivers. SNS is a fully managed pub/sub (publish/subscribe) messaging service, enabling publish messages to topics, which are logical channels or categories.
- **Mechanism
	- Event producer => SNS topics <= receivers listen to SNS topic notifications
		- Producers: CloudWatch, Budgets, Lambda, ASG, S3, DynamoDB, CloudFormation(state changes), DMS(New Replic), RDS events
			- Topic publish
			- Direct Publish
- **Security
	- Encryption
		- In-flight: HTTPS
		- At-rest: KMS
		- Client-side
	- Access Controls: IAM
	- Access Policies: similar to S3
- **Ordering
	- FIFO
- **Message filtering using JSON policy**
	- SNS => filtering policy => different queues for corresponding messages
### SNS + SQS: Fan out
Push once in SNS, receive in all SQS queues that are subscribers. This is a common pattern where only one message is sent to the SNS topic and then "fan-out" to multiple SQS queues. This approach has the following features: it's fully decoupled, no data loss, and you have the ability to add more SQS queues (more applications) over time.
- S3
	- S3 Events => SNS topic => SQS queues
	- SNS => Kinesis Data Firehose => S3
- SNS FIFO + SQS FIFO: Fan out => fan out + ordering + deduplication

SNS facilitates the broadcasting of messages to multiple subscribers through topics, SQS enables the reliable and scalable queuing of messages between different components of a system, helping to decouple components and manage async communication. These two are often used together in AWS architectures to build scalable and resilient distributed applications.
### Kinesis => real-time streaming model

- streaming data v.s. data
	- streaming data is the data generated at this very now. Static/batch data stored and processed in discrete chunks or batches.

A platform for collecting, processing, and analyzing real-time, streaming data, akin to monitoring and managing the flow of information.
- **Data streams
	- Performance
		- The capacity limits of a Kinesis data stream are defined by the number of shards within the data stream. The limits can be exceeded by either data throughput or the number of reading data calls. Each shard allows for 1 MB/s incoming data and 2 MB/s outgoing data. You should increase the number of shards within the data stream to provide enough capacity.
	- Producers: produce records(partition key + data blob) into stream
		- `aws kinesis put-record --stream-name Demostream --partition-key user1 --data "user signup" --cli-binary-format raw-in-base64-out`
		- `aws kinesis describe-stream --stream-name Demostream
		- `aws kinesis get-shard-iterator --stream-name Demostream --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON
		- `aws kinesis get-records --shard-iterator "AAAAAAAAAAG5U/7UVKD+rFGfmq6suBhb2WCaqL+GN9sgXkxhE7vPheX1rpI1Yml5EGyV/JNu5+wlXUGdeKVOo8hUOUGvOE6qw6aRgbdG/yhYX1ow8SOdExxDlIr1t7tu0C3FsMjKteNUR8GBv8LzfI3wCDCrZa9OZ+4GLxlCgnIYgSKzHGn6NAMvDDOsIcdHP8oqYPUn98x1Q5Hh+stuBW+axU3tZeYHl+g8fdcq5nCg9r8PLhRbDA=="`
	- Shards: define capacity
	- Consumers: Receive and consume the records. The consumers can be Apps(KCL, SDK), lambda, Data firehose, Data Analytics...
		- Write you own
	- Capacity Modes
		- Provisioned mode
		- On-demand mode
	- Security: the same as SQS and SNS + VPC, Monitor API calls using CloudTail
- **Data firehose
	- Producers => Firehose => Batch writes to Destinations
		- Destinations
			- AWS: Redshift / S3 / OpenSearch
			- 3rd party partner: Splunk / MongoDB / DataDog / NewRelic
			- Custom: send to any HTTP endpoint
- **Data streams v.s. Data firehose

#### Ordering data 
- into Kinesis
	- Using partition key. The same key will always go to the same shard. Kinesis Data Stream uses the partition key associated with each data record to determine which shard a given data record belongs to. Using the identity of each user as the partition key ensures the data for each user is ordered hence sent to the same shard.
- into SQS FIFO
	- No GroupID, messages are consumed in the order sent, with only one consumer.
	- With GroupID, the number of consumers can be scaled.

#### SQS v.s. SNS v.s. Kinesis

### MQ
A message broker service for communicating between different parts of your application. RabbitMQ, ActiveMQ
Amazon MQ supports industry-standard APIs such as JMS and NMS, and protocols for messaging, including AMQP, STOMP, MQTT, and WebSocket.


Amazon Kinesis Data Streams is well-suited for ingesting and processing large streams of data with variable throughput. Using a single shard allows for simpler processing without the need to manage multiple shards.
