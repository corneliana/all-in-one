## Step Functions

- Model your workflows as state machines (one per workflow)
	- Order fulfillment, Data processing
	- Web applications, Any workflow
- Written in JSON
- Visualization of the workflow and the execution of the workflow, as well as history
- Start workflow with SDK call, API Gateway, EventBridge(CloudWatch Event)

### Task States
- Do some work in your state machine
- Invoke one AWS service
	- Can invoke a Lambda function
	- Run an AWS Batch job
	- Run an ECS task and wait for it to complete
	- Insert an item from DynamoDB
	- Publish message to SNS, SQS
	- Launch another Step Function workflow…
- Run an one Activity
	- EC2, Amazon ECS, on-premises
	- Activities poll the Step functions for work
	- Activities send results back to Step Functions

### States
- Choice State- Test for a condition to send to a branch (or default branch)
- Fail or Succeed State- Stop execution with failure or success
- Pass State- Simply pass its input to its output or inject some fixed data, without performing work.
- Wait State- Provide a delay for a certain amount of time or until a specified time/date.
- Map State- Dynamically iterate steps.’
- _Parallel State__- Begin parallel branches of execution._

### Error handling
- Any state can encounter runtime errors for various reasons:
	- State machine definition issues (for example, no matching rule in a Choice state)
	- Task failures (for example, an exception in a Lambda function)
	- Transient issues (for example, network partition events)
- Use Retry (to retry failed state) and Catch (transition to failure path) in the State Machine to handle the errors instead of inside the Application Code
- Predefined error codes:
	- States.ALL : matches any error name
	- States.Timeout: Task ran longer than TimeoutSeconds or no heartbeat received
	- States.TaskFailed: execution failure
	- States.Permissions: insufficient privileges to execute code
	- The state may report is own errors

### Retry (Task or Parallel State)
- Evaluated from top to bottom
- ErrorEquals: match a specific kind of error
- IntervalSeconds: initial delay before retrying
- BackoffRate: multiple the delay after each retry
- MaxAttempts: default to 3, set to 0 for never retried
- When max attempts are reached, the Catch kicks in

### Catch (Task or Parallel State)
- Evaluated from top to bottom
- ErrorEquals: match a specific kind of error
- Next: State to send to
- ResultPath- A path that determines what input is sent to the state specified in the Next field.

### ResultPath
- Include the error in the input

### Wait for Task Token
- Allows you to pause Step Functions during a Task until a Task Token is returned
- Task might wait for other AWS services, human approval, 3rd party integration, call legacy systems…
- Append .waitForTaskToken to the Resource field to tell Step Functions to wait for the Task Token to be returned
- Task will pause until it receives that Task Token back with a SendTaskSuccess or SendTaskFailure API call
![[wait for task token.png]]

### Activity Tasks
- Task work performed by an Activity Worker which can be running on EC2, lambda, mobile device
- Activity Worker poll for a Task using GetActivityTask API
- After Activity Worker completes its work, it sends a response of its success/failure using SendTaskSuccess or SendTaskFailure
- To keep the Task active:
	- Configure how long a task can wait by setting TimeoutSeconds
	- Periodically send a heartbeat from Activity Worker using SendTaskHeartBeat within the time set in HeartBeatSeconds
- By configuring a long TimeoutSeconds and actively sending a heartbeat, Activity Task can wait up to 1 year

### Standard vs. Express
![[Step function Standard vs. Express.png]]



- AWS Step Functions is a serverless function orchestrator that makes it easy to sequence AWS Lambda functions and multiple AWS services into business-critical applications.
- Through its visual interface, you can create and run a series of checkpointed and event-driven workflows that maintain the application state. The output of one step acts as an input to the next. Each step in your application executes in order, as defined by your business logic.
- AWS Step Functions enables you to implement a business process as a series of steps that make up a workflow. The individual steps in the workflow can invoke a Lambda function or a container that has some business logic, update a database such as DynamoDB or publish a message to a queue once that step or the entire workflow completes execution.

- Benefits of Step Functions:
	- Build and update apps quickly: build visual workflows that enable the fast translation of business requirements into technical requirements. You can build applications in a matter of minutes, and when needs change, you can swap or reorganize components without customizing any code.
	- Improve resiliency: Step Functions manages state, checkpoints and restarts for you to make sure that your application executes in order and as expected. Built-in try/catch, retry and rollback capabilities deal with errors and exceptions automatically.
	- Write less code: Step Functions manages the logic of your application for you and implements basic primitives such as branching, parallel execution, and timeouts. This removes extra code that may be repeated in your microservices and functions.
![[Step Functions.png]]

## AppAsync
- 4 ways to authorize applications to interact with your AWS AppSync GraphQL API
	- API_KEY
	- AWS_IAM: IAM users / roles / cross-account access
	- OPENID_CONNECT: OpenID Connect provider / JSON Web Token
	- AMAZON_COGNITO_USER_POOLS
- For custom domain & HTTPS, use CloudFront in front of AppSync

## AWS Amplify
- Set of tools to get started with creating mobile and web applications:
	- Amplify Studio
	- Amplify CLI
	- Amplify Libraries
	- Amplify Hosting
- “Elastic Beanstalk for mobile and web applications”
- Must-have features such as data storage, authentication, storage, and machine-learning, all powered by AWS services
- Front-end libraries with ready-to-use components for React.js, Vue, Javascript, iOS, Android, Flutter, etc…
- Incorporates AWS best practices to for reliability, security, scalability
- Build and deploy with the Amplify CLI or Amplify Studio

### Important features
- Authentication
	-  Leverages Amazon Cognito
	- User registration, authentication, account recovery & other operations
	- Support MFA, Social Sign-in, etc…
	- Pre-built UI components
	- Fine-grained authorization
- DataStore
	- Leverages Amazon AppSync and Amazon DynamoDB
	- Work with local data and have auto sync to the cloud without complex code
	- Powered by GraphQL
	- Offline and real-time capabilities
	- Visual data modeling w/ Amplify Studio

### Amplify Hosting
- Build and Host Modern Web Apps
- CICD (build, test, deploy)
- Pull Request Previews
- Custom Domains
- Monitoring
- Redirect and Custom Headers
- Password protection

### End-to-End (E2E) Testing
- Run end-to-end (E2E) tests in the test phase in Amplify
- Catch regressions before pushing code to production
- Use the test step to run any test commands at build time (amplify.yml)
- Integrated with Cypress testing framework
- Allows you to generate UI report for your tests