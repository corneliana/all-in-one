
Visual analysis of our applications

## Advantages

## X-Ray Compatibility

## Leverages Tracing

## Enable it

## Magic?

## Troubleshooting

## Instrumentation in code

## X-Ray Concepts

- Segments: each application / service will send them
- Subsegments: if you need more details in your segment
- Trace: segments collected together to form an end-to-end trace
- Sampling: decrease the amount of requests sent to X-Ray, reduce cost
- Annotations: Key Value pairs used to index traces and use with filters
- Metadata: Key Value pairs, not indexed, not used for searching
- The X-Ray daemon / agent has a config to send traces cross account:
	- make sure the IAM permissions are correct – the agent will assume the role
	- This allows to have a central account for all your application tracing

## Sampling Rules

- With sampling rules, you control the amount of data that you record
- You can modify sampling rules without changing your code
- By default, the X-Ray SDK records the first request each second, and five percent of any additional requests.
- One request per second is the _reservoir_, which ensures that at least one trace is recorded each second as long the service is serving requests.
- Five percent is the _rate_ at which additional requests beyond the reservoir size are sampled.

## Custom Sampling Rules

## Write APIs (used by the X-Ray daemon)


## with Elastic Beanstalk

## ECS + X-Ray integration options

## AWS Distro for OpenTelemetry