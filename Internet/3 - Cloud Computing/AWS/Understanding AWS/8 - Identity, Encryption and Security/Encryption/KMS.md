- **KMS: Manage encryption keys
	- A highly secure vault to keep all the encryption keys for resources. Only those with permission can access these keys to unlock or secure the valuable information.
	- Fully integrated with IAM for authorization
	- Able to audit KMS Key usage using CloudTrail
	- Seamlessly integrated into most AWS services (EBS, S3, RDS, SSM...)
	- Automatic Rotation on your KMS Key, the backing key is rotated every 1 year
	- Never ever store your secrets in plaintext, especially in your code!
		- KMS Key Encryption also available through API calls (SDK, CLI)
		- Encrypted secrets can be stored in the code / environment variables
	- Keys type
		- Symmetric(AES-256 keys)
			- AWS services that are integrated with KMS use **Symmetric CMKs**
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
	- Multi-Region Keys: Primary key => Replica key
		- Identical KMS keys in different AWS Regions that can be used interchangeably
		- Multi-Region keys have the same key ID, key material, automatic rotation...
		- Encrypt in one Region and decrypt in other Regions
		- Each Multi-Region key is managed independently
		- Use cases: global client-side encryption, encryption on Global DynamoDB, Global Aurora
	- S3 Replication Encryption Considerations
		- Unencrypted objects and objects encrypted with SSE-S3 are replicated by default
		- Objects encrypted with SSE-C (customer provided key) can be replicated
		- Objects encrypted with SSE-KMS, need to enable the option
			- Specify which KMS Key to encrypt the objects within the target bucket
			- Adapt the KMS Key Policy for the target key  
			- An IAM Role with `kms:Decrypt` for the source KMS Key and `kms:Encrypt` for the target KMS Key
			- Might get KMS throttling errors, so can ask for a Service Quotas increase
		- Can use multi-region AWS KMS Keys, but they are now treated as independent keys by S3 (the object will still be decrypted and then encrypted)
	- AMI Sharing Process Encrypted via KMS
		- AMI in Source Account is encrypted with KMS Key from Source Account
		- Must modify the image attribute to add a Launch Permission which corresponds to the specified target AWS account
		- Must share the KMS Keys used to encrypted the snapshot the AMI references with the target account / IAM Role
		- The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt
		- When launching an EC2 instance from the AMI, optionally the target account can specify a new KMS key in its own account to re-encrypt the volumes