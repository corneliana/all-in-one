## Identity
- [[IAM Identity and Access Management]]
- [[Cognito]]

## Security
- [[Security Groups]]
- [[SSM Parameter Store]]

## ACL
Access Control Lists (ACLs) are used to **manage permissions at a resource level within AWS services** like S3. ACLs are not used to control access across multiple AWS accounts at the organizational level.



## Data Security

### AWS Secrets Manager
storing and managing sensitive information such as database credentials.
specifica
lly designed for managing secrets like database credentials. It provides additional features tailored for secret management, such as integration with RDS for automatic rotation, and it's generally considered the best practice for managing credentials securely.

### AWS Systems Manager Parameter Store
storing and managing sensitive information such as database credentials.
a versatile service primarily used for storing configuration data and secrets. While it does support automatic rotation of parameters, it's not specifically tailored for secret management like AWS Secrets Manager. Parameter Store may be suitable for managing other types of configuration data but may lack some of the advanced features and integrations provided by Secrets Manager for managing secrets like database credentials.





