## SSM Parameter Store
- **SSM Parameter Store: Securely store parameters and secrets in an organized way**
	- Secure storage for configuration and secrets
	- Optional Seamless Encryption using KMS
	- Serverless, scalable, durable, easy SDK
	- **Version tracking of configurations / secrets**
	- Security through IAM
	- Notifications with Amazon EventBridge
	- Integration with CloudFormation
	- Parameter Policy(advanced param)
		- Allow to assign a TTL to a parameter (expiration date) to force updating or deleting sensitive data such as passwords
		- Can assign multiple policies at a time
![[Pasted image 20241123001208.png]]

### Parameters Policies (for advanced parameters)
- Allow to assign a TTL to a parameter (expiration date) to force updating or deleting sensitive data such as passwords
- Can assign multiple policies at a time

## Secrets Manager
- Newer service to store secrets(encrypted using KMS)
- **Force rotation of secrets every X days** and automate generating secrets on rotation using **lambda**
- **Integration with RDS(MySQL, PostgreSQL, Aurora)** and **mostly meant for RDS and Aurora integration**
- Multi-Region Secrets
		- **Replicate Secrets across multiple AWS Regions**
		- Secrets Manager keeps read replicas in sync with the primary Secret
		- Ability to promote a read replica Secret to a standalone Secret
		- Use cases: multi-region apps, disaster recovery strategies, multi-region DB...

## SSM Parameter Store v.s. Secrets Manager
- Secrets Manager ($$$):
	- Automatic rotation of secrets with AWS Lambda
	- Lambda function is provided for RDS, Redshift, DocumentDB
	- KMS encryption is mandatory
	- Can integration with CloudFormation

- SSM Parameter Store ($):
	- Simple API
	- No secret rotation (can enable rotation using Lambda triggered by EventBridge)
	- KMS encryption is optional
	- Can integration with CloudFormation
	- Can pull a Secrets Manager secret using the SSM Parameter Store API

![[Pasted image 20241123001556.png]]

## CloudFormation - Dynamic References
- Reference external values stored in Systems Manager Parameter Store and Secrets Manager within CloudFormation templates
- CloudFormation retrieves the value of the specified reference during create/update/delete operations
- For example: retrieve RDS DB Instance master password from Secrets Manager
- Supports
	- ssm – for plaintext values stored in SSM Parameter Store
	- ssm-secure – for secure strings stored in SSM Parameter Store
	- secretsmanager – for secret values stored in Secrets Manage