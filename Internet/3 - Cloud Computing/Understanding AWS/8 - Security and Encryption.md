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


- **Security groups**: Act as a virtual firewall for your EC2 instances.
- **KMS (Key Management Service)**: Manage encryption keys.
- **SSM Parameter Store**: Securely store parameters and secrets.
- **Shield**: Managed Distributed Denial of Service (DDoS) protection service.
- **WAF (Web Application Firewall)**: Protect web applications from common web exploits.


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

## GuardDuty
Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior in AWS accounts and workloads. It is not specifically designed for traffic inspection or filtering within a VPC. GuardDuty focuses on identifying threats and anomalies based on network traffic, AWS API calls, and DNS data.

## Firewall Manager
AWS Firewall Manager is a security management service that allows you to centrally configure and manage firewall rules across multiple AWS accounts and resources. While it can help enforce security policies and manage firewall rules, it is not specifically designed for traffic inspection and filtering within a single VPC.