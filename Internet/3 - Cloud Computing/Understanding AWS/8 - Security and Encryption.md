
## Encryption
- **In flight(SSL)**
	- Data is encrypted before sending and decrypted after receiving 
	- SSL certificates help with encryption (HTTPS)
	- Encryption in flight ensures no MITM (man in the middle attack) can happen
- SSE at rest
	- Data is encrypted after being received by the server, and decrypted before sent
	- The encryption / decryption keys must be managed somewhere and the server must have access to it
- CSE
	- Data is encrypted by client and never decrypted by the server but by a receiving client
	- Could leverage Envelope Encryption

- KMS: Manage encryption keys
	- Fully integrated with IAM for authorization
	- Able to audit KMS Key usage using CloudTrail
	- Seamlessly integrated into most AWS services (EBS, S3, RDS, SSM...)
	- Never ever store your secrets in plaintext, especially in your code!
		- KMS Key Encryption also available through API calls (SDK, CLI)
		- Encrypted secrets can be stored in the code / environment variables
	- Keys type
		- Symmetric(AES-256 keys)
			- AWS services that are integrated with KMS use Symmetric CMKs
			- You never get access to the KMS Key unencrypted (must call KMS API to use)
		- Asymmetric
			- Public (Encrypt) and Private Key (Decrypt) pair
			- Used for Encrypt/Decrypt, or Sign/Verify operations
			- The public key is downloadable, but can’t access the Private Key unencrypted
			- Use case: encryption outside of AWS by users who can’t call the KMS API
		- Types
			- AWS owned/managed, customer managed keys created/imported
		- Polices
			- Similar to S3 bucket policies
	
- Multi-Region Keys
- Secrets Manager
- Certificate Manager(ACM)
	- Requesting Public Certificates
- API Gateway
- WAF: Web Application Firewall
	- Protect web applications from common web exploits.
- Shield: Managed Distributed Denial of Service (DDoS) protection service.
- Firewall Manager
	- A security management service that centrally configure and manage firewall rules across multiple AWS accounts and resources. While it can help enforce security policies and manage firewall rules, it is not specifically designed for traffic inspection and filtering within a single VPC.
- GuardDuty
	- A threat detection service that continuously monitors for malicious activity and unauthorized behavior in AWS accounts and workloads. It is not specifically designed for traffic inspection or filtering within a VPC. It focuses on identifying threats and anomalies based on network traffic, AWS API calls, and DNS data.
- Inspector
	- 
- Macia
	- A fully managed data security and data privacy service that **uses machine learning and pattern matching to discover and protect sensitive data** in AWS.

## IAM: Identity and Access Management
Secure control access to AWS services and resources.

- **Condition keys**
	- `aws:PrincipalOrgID` control access based on the organization ID of the AWS account making the request. 
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowGetObjectForOrgMembers",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgID": "o-1234567890"
        }
      }
    }
  ]
}

```
For example, the S3 bucket policy allows `s3:GetObject` action for objects in the `example-bucket` bucket only if the requester's AWS account is a member of the organization with the ID `o-1234567890`. This ensures that only users or roles from accounts within the specified organization can access the objects in the bucket.

- `aws:PrincipalOrgPaths`: control access based on the organizational unit (OU) path of the AWS account making the request within the AWS Organization.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowGetObjectForOrgMembers",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgPaths": "ou-abc123/ou-def456"
        }
      }
    }
  ]
}
```
In this example, the S3 bucket policy allows `s3:GetObject` action for objects in the `example-bucket` bucket only if the requester's AWS account is located within the OU path `ou-abc123/ou-def456` within the AWS Organization.

- `aws:PrincipalTag`: control access based on the tags attached to the principal making the request.
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAccessBasedOnPrincipalTag",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::example-bucket/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalTag/Department": "Marketing"
        }
      }
    }
  ]
}

```
In this example, the S3 bucket policy allows `s3:GetObject` action for objects in the `example-bucket` bucket only if the requester's principal has the tag `Department:Marketing` attached to it. This condition ensures that only principals with the specified tag can access the objects in the bucket.

- Organizations
	- Global service to manage multiple AWS accounts

- IAM roles v.s. Resource-based policies
	- When you assume a role (user, application or service), you give up your original permissions and take the permissions assigned to the role
	- Resource-based policy: Lambda, SNS, SQS, CloudWatch Logs, API Rule Gateway...
	- IAM role: Kinesis stream, Systems Manager Run Command, ECS task...

- Permission boundaries
	- IAM permission boundaries complement IAM users or roles by setting a limit on the maximum permissions that can be assigned to them.

- Identity Center(successor to AWS single sign-on): Fine grained Permissions and Assignments
	- One login (single sign-on) for all your  
		- AWS accounts in AWS Organizations  
		- Business cloud applications (e.g., Salesforce, Box, Microsoft 365, ...) 
		- SAML2.0-enabled applications  
		- EC2 Windows Instances

- Control Tower
	- Easy way to set up and govern a secure and compliant multi-account AWS environment based on best practices

## Security groups
Act as a virtual firewall for your EC2 instances.


## SSM Parameter Store
Securely store parameters and secrets.


## Cognito
Amazon Cognito can be used to federate mobile user accounts and provide them with their own IAM permissions, so they can be able to access their own personal space in the S3 bucket.

Amazon Cognito lets you add user sign-up, sign-in, and access control to web and mobile apps quickly and easily. It scales to millions of users and supports sign-in with social identity providers, such as Apple, Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0 and OpenID Connect.


## Data Security

### AWS Secrets Manager
storing and managing sensitive information such as database credentials.
specifica
lly designed for managing secrets like database credentials. It provides additional features tailored for secret management, such as integration with RDS for automatic rotation, and it's generally considered the best practice for managing credentials securely.

### AWS Systems Manager Parameter Store
storing and managing sensitive information such as database credentials.
a versatile service primarily used for storing configuration data and secrets. While it does support automatic rotation of parameters, it's not specifically tailored for secret management like AWS Secrets Manager. Parameter Store may be suitable for managing other types of configuration data but may lack some of the advanced features and integrations provided by Secrets Manager for managing secrets like database credentials.

### Macia
A security service to help organizations discover, classify, and protect sensitive data stored in AWS. It uses machine learning and pattern matching techniques to automatically identify and classify sensitive data such as personally identifiable information (PII), intellectual property, and financial data. Macie can analyze data stored in various AWS services, including S3 buckets, Amazon RDS databases, and AWS Glue Data Catalogs.



