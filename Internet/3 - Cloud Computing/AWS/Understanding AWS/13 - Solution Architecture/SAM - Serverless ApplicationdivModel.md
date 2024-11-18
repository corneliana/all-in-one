- SAM = Serverless Application Model
- Framework for developing and deploying serverless applications
- All the configuration is **YAML code**
- Generate complex CloudFormation from simple SAM YAML file
- Supports anything from CloudFormation: Outputs, Mappings, Parameters, Resources…
- SAM can use CodeDeploy to deploy Lambda functions
- SAM can help run Lambda, API Gateway, DynamoDB locally

## Recipe
- Transform Header indicates it’s SAM template:
	- Transform: 'AWS::Serverless-2016-10-31'
- Write Code
	- AWS::Serverless::Function
	- AWS::Serverless::Api
	- AWS::Serverless::SimpleTable
- Package & Deploy: sam deploy (optionally preceded by “sam package”)
- Quickly sync local changes to AWS Lambda (SAM Accelerate): sam sync --watch

## SAM Deployment
![[SAM Deployment.png]]

## SAM Accelerate (sam sync)
- SAM Accelerate is a set of features to **reduce latency** while deploying resources to AWS
- _sam sync_
	- Synchronizes the project declared in SAM templates to AWS
	- Synchronizes code changes to AWS without updating infrastructure (uses service APIs & bypass CloudFormation)
- Examples
	- sam sync (no options)
		- Synchronize code and infrastructure
	- sam sync --code
		- Synchronize code changes without updating infrastructure (bypass CloudFormation, update in seconds)
	- sam sync --code --resource AWS::Serverless::Function
		- Synchronize only all Lambda functions and their dependencies
	- sam sync --code --resource-id HelloWorldLambdaFunction
		- Synchronize only a specific resource by its ID
	- sam sync --watch
		- Monitor for file changes and automatically synchronize when changes are detected
		- If changes include configuration, it uses sam sync
		- If changes are code only, it uses sam sync --code
## SAM – CLI Debugging
- Provides a lambda-like execution environment locally
- SAM CLI + AWS Toolkits => step-through and debug your code

## SAM Policy Templates
- Important examples:
	- S3ReadPolicy: Gives read only permissions to objects in S3
	- SQSPollerPolicy: Allows to poll an SQS queue
	- DynamoDBCrudPolicy: CRUD = create read update delete

## SAM and CodeDeploy
- SAM framework natively uses CodeDeploy to update Lambda functions
- Pre and Post traffic hooks features to validate deployment (before the traffic shift starts and after it ends)
- Easy & automated rollback using CloudWatch Alarms

## Local Capabilities
- Locally start AWS Lambda 
	- _sam local start-lambda_
	- Starts a local endpoint that emulates AWS Lambda
	- Can run automated tests against this local endpoint
- Locally Invoke Lambda Function
	- _sam local invoke_
	- Invoke Lambda function with payload once and quit after invocation completes 
	- Helpful for generating test cases
	- If the function make API calls to AWS, make sure you are using the correct --_profile_ option
- Locally Start an API Gateway Endpoint
	- _sam local start-api_
	- Starts a local HTTP server that hosts all your functions
	- Changes to functions are automatically reloaded
- Generate AWS Events for Lambda Functions
	- _sam local generate-event_
	- Generate sample payloads for event sources
	- S3, API Gateway, SNS, Kinesis, DynamoDB…