
We have learned how to:
- Create AWS resources, manually (fundamentals)
- Interact with AWS programmatically (AWS CLI)
- Deploy code to AWS using Elastic Beanstalk
All these manual steps make it very likely for us to do mistakes!

We would like the code “in a repository” and have it deployed onto AWS
- Automatically
- The right way
- Making sure it’s tested before being deployed
- With possibility to go into different stages (dev, test, staging, prod)
- With manual approval where needed
- To be a proper AWS developer… we need to learn AWS CICD

## Introduction
- AWS CodeCommit – **storing** our code
- AWS CodePipeline – automating the pipeline from code to Elastic Beanstalk
- AWS CodeBuild – building and testing the code
- AWS CodeDeploy – deploying the code to EC2 instances (not Elastic Beanstalk)
- AWS CodeStar – manage software development activities in one place
- AWS CodeArtifact – store, publish, and share software packages
- AWS CodeGuru – automated code reviews using Machine Learning

## CI: Continuous Integration
- Developers push the code to a code repository often (e.g., GitHub, CodeCommit, Bitbucket…)
- A testing / build server checks the code as soon as it’s pushed (CodeBuild, Jenkins CI, …)
- The developer gets feedback about the tests and checks that have passed / failed

## CD: Continuous Delivery
- Ensures that the software can be released reliably whenever needed
- Ensures deployments happen often and are quick
- Shift away from “one release every 3 months” to ”5 releases a day”
- That usually means automated deployment (e.g., CodeDeploy, Jenkins CD, Spinnaker, …)

## Technology Stack for CICD
![[CICD.png]]


## AWS CodeCommit – **storing** our code - Version Control
***On July 25th 2024, AWS abruptly discontinued CodeCommit***

- Git repositories can be expensive
- The industry includes GitHub, GitLab, Bitbucket, …
- And AWS CodeCommit:
	- Private Git repositories
	- No size limit on repositories (scale seamlessly)
	- Fully managed, highly available
	- Code only in AWS Cloud account => increased security and compliance
	- Integrated with Jenkins, AWS CodeBuild, and other CI tools
	- Security (encrypted, access control, …)
		- Interactions are done using Git
		- Authentication
			- SSH Keys – AWS Users can configure SSH keys in their IAM Console
			- HTTPS – with AWS CLI Credential helper or Git Credentials for IAM user
		- Authorization
			- IAM policies to manage users/roles permissions to repositories
		- Encryption
			- Repositories are automatically encrypted at rest using AWS KMS
			- Encrypted in transit (can only use HTTPS or SSH – both secure)
		- Cross-account Access
			- Do NOT share your SSH keys or your AWS credentials
			- Use an IAM Role in your AWS account and use AWS STS (AssumeRole API)

| Feature                             | CodeCommit              | GitHub                                                                 |
| ----------------------------------- | ----------------------- | ---------------------------------------------------------------------- |
| Support Code Review (Pull Requests) | ✓                       | ✓                                                                      |
| Integration with AWS CodeBuild      | ✓                       | ✓                                                                      |
| Authentication (SSH & HTTPS)        | ✓                       | ✓                                                                      |
| Security                            | IAM Users & Roles       | GitHub Users                                                           |
| Hosting                             | Managed & hosted by AWS | - Hosted by GitHub<br>- GitHub Enterprise: self hosted on your servers |
| UI                                  | Minimal                 | Fully Featured                                                         |

## AWS CodePipeline
Visual Workflow to orchestrate the CICD, automating the pipeline from code to Elastic Beanstalk
- Source – CodeCommit, ECR, S3, Bitbucket, GitHub
- Build – CodeBuild, Jenkins, CloudBees, TeamCity
- Test – CodeBuild, AWS Device Farm, 3rd party tools, …
- Deploy – CodeDeploy, Elastic Beanstalk, CloudFormation, ECS, S3, …
- Invoke – **Lambda, Step Functions**
- Consists of stages:
	- Each stage can have sequential actions and/or parallel actions
	- Example: Build => Test => Deploy => Load Testing => …
	- Manual approval can be defined at any stage

### Artifacts
- Each pipeline stage can create artifacts
- Artifacts stored in an S3 bucket and passed on to the next stage
![[CodePipeline - Artifacts.png]]

### Troubleshooting
 - Use CloudWatch Events (Amazon EventBridge). Example:
	 - create events for failed pipelines
	 - create events for cancelled stages
- If CodePipeline fails a stage, your pipeline stops, and you can get information in the console
- If pipeline can’t perform an action, make sure the “IAM Service Role” attached does have enough IAM permissions (IAM Policy)
- AWS CloudTrail can be used to audit AWS API calls

## AWS CodeBuild – building and testing the code
- Compile source code, run tests, produce software packages, …
- Charged per minute for compute resources (time it takes to complete the builds)
- Leverages Docker under the hood for reproducible builds
- Use prepackaged Docker images or create your own custom Docker image
- Security:
	- Integration with KMS for encryption of build artifacts
	- IAM for CodeBuild permissions, and VPC for network security
	- AWS CloudTrail for API calls logging

- **CodeBuild containers are deleted at the end of their execution (success or failure). You can't SSH into them, even while they're running.**
- **CodeBuild can run any commands, so you can use it to run commands including build a static website and copy your static web files to an S3 bucket.**
- You can configure CodeBuild to run its build containers in a VPC, so they can access private resources in a VPC such as databases, internal load balancers, ...

- `buildspec.yml` file must be at the root of your code
- env – define environment variables
	- variables – plaintext variables
	- parameter-store – variables stored in SSM Parameter Store
	- secrets-manager – variables stored in AWS Secrets Manager
- phases – specify commands to run:
	- install – install dependencies you may need for your build
	- pre_build – final commands to execute before build
	- Build – actual build commands
	- post_build – finishing touches (e.g., zip output)
- artifacts – what to upload to S3 (encrypted with KMS)
- cache – files to cache (usually dependencies) to S3 for future build speedup

## AWS CodeDeploy
deploying the code to:
- EC2 instances (not Elastic Beanstalk) / On-premises servers
	- in-placement deployments or blue green deployment
- CodeDeployAgent 
	- running on EC2 instances
	- get deployment bundles from S3 buckets
- Lambda
	- automate traffic shift for lambda alias
	- linear: grow traffic every N mins until 100%
	- Canary: X% => 100%
	- AllAtOnce
- ECS
	- Only blue-green deployments
	- linear: grow traffic every N mins until 100%
	- Canary: X% => 100%
	- AllAtOnce

- Hooks can be put for Deployment phases
- Rollback: redeploy the last good deployment
## AWS CodeStar
manage software development activities in one place

## AWS CodeArtifact
store, publish, and share software packages
- Software packages depend on each other to be built (code dependencies), and new ones are created. Storing and retrieving these dependencies is called artifact management
- CodeArtifact is a secure, scalable, and cost-effective artifact management for software development. Works with common dependency management tools such as Maven, Gradle, npm, yarn, twine, pip, and NuGet
- Developers and CodeBuild can then retrieve dependencies straight from CodeArtifact

![[CodeArtifact.png]]

![[CodeArtifact - EventBridge Integration.png]]

## Resource Policy
- Can be used to authorize another account to access CodeArtifact
- A given principal can either read all the packages in a repository or none of them

CodeArtifact v.s. Functional Programming
Common pattern:
- Each stage processes output from previous stage
- Dependencies are explicit
- Flow of data through transformations
- Immutable artifacts between stages

Main difference:
- FP: Transforms data in memory
- CI/CD: Transforms artifacts between stages
## AWS CodeGuru
- automated code reviews and application performance recommendations using Machine Learning
- Agent Configuration
	- MaxStackDepth
	- MemoryUsageLimitPercent
	- MinimumTimeForReportingInMilliseconds
	- ReportingIntervalInMilliseconds
	- SamplingIntervalMilliseconds