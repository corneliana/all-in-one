## Kafka

A messaging service to process real-time data.
Streaming: a continuous flow, processed on the go (real-time data processing), 
- i.e. Event Driven Architecture (EDA) approach
- event broker: intermediaries between producers and consumers to transmit events in a publish-subscribe pattern.

event and topics => permission => Kafka topics => permissions => BQ / ElasticSearch / Application / S3 Buckets

- Data: the message or payload you want to send or receive through Kafka
- Event: "what happened" or "what you want to represent" with the data you send
- Topic/event streams: categorize and organize these events, making it easier for consumers to subscribe to and process relevant events. 
	- A topic can be understood as being analogous to a database table at times with the exception that it is immutable.
	- An immutable log of events to be processed now or later by an event consumer.
The flow in Kafka: producing events (as data messages) to topics, where each event represents a significant occurrence you're interested in. Consumers then read from these topics to receive and process the events.


Apache Kafka, Apache Pulsar and Google Pub/Sub
Complete offerings like Apache Kafka, Apache Pulsar and Google Pub/Sub provide a variety of rich capabilities beyond just storage of the event payload ranging from distributed transaction management, geo-replication, keyed-ordering, tiered storage, partitioning, idempotency, etc. which greatly simplify working in an evented paradigm.

SQS and SNS