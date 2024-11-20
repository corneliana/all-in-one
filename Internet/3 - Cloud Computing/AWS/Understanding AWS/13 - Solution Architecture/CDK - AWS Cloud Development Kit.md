- Define your cloud infrastructure using a familiar language:
	- JavaScript/TypeScript, Python, Java, and .NET
- Contains high level components called constructs
- The code is "compiled" into a CloudFormation template(JSON/YML)
- Therefore can **deploy infrastructure and application runtime code together**
	- Great for lambda functions
	- Great for Docker containers in ECS/EKS

## Diagram
![[CDK Diagram.png]]
## CDK v.s. SAM
- SAM:
	- Serverless focused
	- Write template declaratively in JSON or YAML
	- Great for quickly getting started with Lambda
	- Leverages CloudFormation
- CDK: Superset of CloudFormation
	- All AWS services
	- Write infra in a programming language JavaScript/TypeScript, Python, Java, and .NET
	- Leverages CloudFormation

## CDK + SAM
- Can use SAM CLI to locally test CDK apps
- You must first run _cdk synth_
![[CDK + SAM.png]]

## CDK hands-on
![[CDK hands on.png]]
1. Initialize the application: `cdk init app --language javascript`
2. verify it works property: `cdk ls`
3. copy the contents of cdk-app-stack.js into lib/cdk-app-stack.js
4. setup the lambda function
	1. `mkdir lambda && cd lambda`
	2. `touch index.py`
5. bootstrap the CDK application: `cdk bootstrap`
6. (optional) synthesize as a CloudFormation template: `cdk synth`
7. deploy the CDK stack: `cdk deploy`
8. empty the s3 bucket
9. destroy the stack: `cdk destroy`

## CDK Constructs
- CDK Construct is a **component that encapsulates everything CDK needs to create the final CloudFormation stack**
- Can represent a single AWS resource (e.g., S3 bucket) or multiple related resources (e.g., worker queue with compute)
- AWS Construct Library
	- A collection of Constructs included in AWS CDK which contains Constructs for every AWS resource
	- Contains 3 different levels of Constructs available (L1, L2, L3)
	- Construct Hub – contains additional Constructs from AWS, 3rd parties, and open-source CDK community

### Lay 1 Constructs
#### CloudFormation level: raw mapping to CF, basic resource creation, but still verbose
- Can be called _CFN Resources_ which represents all resources directly available in CloudFormation
- Constructs are periodically generated from CloudFormation Resource Specification
- Construct names start with Cfn (e.g., CfnBucket)
- You must explicitly configure all resource properties

### Lay 2 Constructs
#### AWS resource level: adds sensible defaults and simpler API
- Represents AWS resources but with a higher level (intent-based API)
- Similar functionality as L1 but with convenient defaults and boilerplate
- You don’t need to know all the details about the resource properties
- Provide methods that make it simpler to work with the resource (e.g., bucket.addLifeCycleRule())

### Lay 3 Constructs
#### Solution level: implements entire architecture patterns, complex architectural patterns with minimal code
- Can be called **_Patterns**,_ which represents multiple related resources
- Helps you complete common tasks in AWS
- Examples:
	- _aws-apigateway.LambdaRestApi_ represents an API Gateway backed by a Lambda function
	- _aws-ecs-patterns.ApplicationLoadBalancerFargateService_ which represents an architecture that includes a Fargate cluster with Application Load Balancer

## Bootstraping


## Testing
