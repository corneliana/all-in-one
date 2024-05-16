## Serverless

**Serverless Solution Architecture**
- TODO APP
	- REST API layer: API gateway => lambda => DynamoDB
	- giving user access to S3: use Amazon Cognito and then S3 with temporary credentials
	- high read throughput, static data: 
		- DAX as a caching layer between lambda and DynamoDB
		- Caching at the API Gateway

- Serverless hosted website: MyBlog.com
	- Server static content globally: CloudFront => S3
	- Server static content globally, securely: CloudFront => OAC => S3
	- Add a public serverless REST API: API gateway => lambda => DAX => DynamoDB Global Tables
	- User welcome email: DynamoBD Stream => invoke lambda(IAM role) => SES
	- Users upload image and thumbnail generated: upload => CloudFront(S3 transfer acceleration) => S3 => lambda => create and put thumbnail in another S3 => SQS/SNS

- Summary
	- We’ve seen static content being distributed using CloudFront with S3 • The REST API was serverless, didn’t need Cognito because public  
	- We leveraged a Global DynamoDB table to serve the data globally  
	- (we could have used Aurora Global Database)
	- We enabled DynamoDB streams to trigger a Lambda function  
	- The lambda function had an IAM role which could use SES  
	- SES (Simple Email Service) was used to send emails in a serverless way
	- S3 can trigger SQS / SNS / Lambda to notify of events

**Microservices architecture**
- You are free to design each micro-service the way you want
- Synchronous patterns: API Gateway, Load Balancers  
- Asynchronous patterns: SQS, Kinesis, SNS, Lambda triggers (S3)
- Challenges with micro-services:  
	- repeated overhead for creating each new microservice,  
	- issues with optimizing server density/utilization  
	- complexity of running multiple versions of multiple microservices simultaneously  
	- proliferation of client-side code requirements to integrate with many separate services.
- Some of the challenges are solved by Serverless patterns:  
	- API Gateway, Lambda scale automatically and you pay per usage  
	- You can easily clone API, reproduce environments  
	- Generated client SDK through Swagger integration for the API Gateway

**Software update distribution**
- Add CloudFront
	- No changes to architecture  
	- Will cache software update files at the edge  
	- Software update files are not dynamic, they’re static (never changing)
	- Our EC2 instances aren’t serverless  
	- But CloudFront is, and will scale for us  
	- Our ASG will not scale as much, and we’ll save tremendously in EC2
	- We’ll also save in availability, network bandwidth cost, etc  
	- Easy way to make an existing application more scalable and cheaper! if it's mostly static content just using some caching at the edges all around the world.