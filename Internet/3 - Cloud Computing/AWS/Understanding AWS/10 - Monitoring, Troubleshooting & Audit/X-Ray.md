
Visual analysis of the applications.
- Collects data from all the different services.
- Service map is computed from all the segments and traces.
- Graphical, so even non technical people can help troubleshoot.

## Advantages
- Troubleshooting performance (bottlenecks)
- Understand dependencies in a microservice architecture
- Pinpoint service issues
- Review request behavior
- Find errors and exceptions
- Are we meeting time SLA?
- Where I am throttled?
- Identify users that are impacted

## X-Ray Compatibility
- AWS Lambda
- Elastic Beanstalk
- ECS
- ELB
- API Gateway
- EC2 Instances or any application server (even on premise)

## Leverages Tracing
- End to end way to follow a request
- Each component dealing with the request along the way adds its own trace.

## Enable it
- Codebase - install X-Ray SDK
- Install the X-Ray daemon or enable X-Ray AWS integration

## Troubleshooting
- If X-Ray is not working on EC2
	- Ensure the EC2 IAM Role has the proper permissions
	- Ensure the EC2 instance is running the X-Ray Daemon
- To enable on AWS Lambda:
	- Ensure it has an IAM execution role with proper policy (AWSX-RayWriteOnlyAccess)
	- Ensure that X-Ray is imported in the code
	- Enable Lambda **X-Ray Active Tracing**

## Instrumentation in code
- Instrumentation means the measure of product's performance, diagnose errors and to write trace information

## X-Ray Concepts
- Segments: each application / service will send them
- Subsegments: if you need more details in your segment
- Trace: segments collected together to form an end-to-end trace(of the API call)
- Sampling: decrease the amount of requests sent to X-Ray, reduce cost
- Annotations: **Key Value pairs used to index traces and use with filters => the ability to search and filter through information efficiently**
- Metadata: Key Value pairs, not indexed, not used for searching
- The X-Ray daemon / agent has a config to send traces cross account:
	- make sure the IAM permissions are correct – the agent will assume the role
	- This allows to have a central account for all your application tracing

## Sampling Rules
- With sampling rules, you control the amount of data that you record
- You can modify sampling rules without changing your code
- By default, the X-Ray SDK records the first request each second, and five percent of any additional requests.
- One request per second is the **_reservoir_**, which ensures that at least one trace is recorded each second as long the service is serving requests.
- Five percent is the **_rate_** at which additional requests beyond the reservoir size are sampled.

## Custom Sampling Rules
- Customize with the reservoir and rate

## Write APIs (used by the X-Ray daemon)
- PutTraceSegments: upload segment docs to X-Ray
- PutTelemetryRecords: upload telemetry
- GetSamplingRules: Retrieve all sampling rules(to know what and when to send)

## Read APIs - continued
- GetServiceGraph
- BatchGetTraces
- GetTraceSummaries
- GetTraceGraph

## With Elastic Beanstalk
- Beanstalk include X-Ray daemon
- run the daemon by setting an option in the Elastic Beanstalk console or with a configuration file (in .ebextensions/xray-daemon.config)
- instance profile has the correct IAM permissions
- application code is instrumented with the X-Ray SDK
- X-Ray daemon is not provided for Multicontainer Docker

## ECS + X-Ray integration options
![[ECS + X-Ray integration options.png]]

![[ECS + X-Ray integration options Example.png]]

## AWS Distro for OpenTelemetry
- Secure, production-ready AWS-supported **distribution** of the open-source project OpenTelemetry project
- Provides **a single set of APIs, libraries, agents, and collector services**
- Collects distributed traces and metrics from your apps
- Collects metadata from your AWS resources and services
- Auto-instrumentation Agents to collect traces without changing your code
- Send traces and metrics to multiple AWS services and partner solutions
	- X-Ray, CloudWatch, Prometheus…
- Instrument your apps running on AWS (e.g., EC2, ECS, EKS, Fargate, Lambda) as well as on-premises
- Migrate from X-Ray to AWS Distro for Temeletry if you want to standardize with open-source APIs from Telemetry or send traces to multiple destinations simultaneously
![[AWS Distro for OpenTelemetry.png]]