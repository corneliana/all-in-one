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