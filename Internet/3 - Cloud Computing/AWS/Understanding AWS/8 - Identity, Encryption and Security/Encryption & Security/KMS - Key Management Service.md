## KMS: Manage encryption keys
- A highly secure vault to keep all the encryption keys for resources. Only those with permission can access these keys to unlock or secure the valuable information.
	- Fully integrated with IAM for authorization
	- Able to audit KMS Key usage using CloudTrail
	- Seamlessly integrated into most AWS services (EBS, S3, RDS, SSM...)
	- Automatic Rotation on your KMS Key, the backing key is rotated every 1 year
	- Never ever store your secrets in plaintext, especially in your code!
		- KMS Key Encryption also available through API calls (SDK, CLI)
		- Encrypted secrets can be stored in the code / environment variables

## How does KMS work? API - Encrypt and Decrypt
![[How does KMS work.png]]

## Keys type
### Symmetric(AES-256 keys)
- AWS services that are integrated with KMS use **Symmetric CMKs**
- You never get access to the KMS Key unencrypted (must call KMS API to use)

- Encrypt: encrypt up to 4 KB of data through KMS
- GenerateDataKey: generates a unique symmetric data key (DEK)
	- returns a plaintext copy of the data key
	- AND a copy that is encrypted under the CMK that you specify
- GenerateDataKeyWithoutPlaintext:
	- Generate a DEK to use at some point (not immediately)
	- DEK that is encrypted under the CMK that you specify (must use Decrypt later)
- Decrypt: decrypt up to 4 KB of data (including Data Encryption Keys)
- GenerateRandom: Returns a random byte string
### Asymmetric
- Public (Encrypt) and Private Key (Decrypt) pair
- Used for Encrypt/Decrypt, or Sign/Verify operations
- The public key is downloadable, but can’t access the Private Key unencrypted
- Use case: encryption outside of AWS by users who can’t call the KMS API
	
## Types of KMS keys
- AWS owned keys: free, SSE-S3, SSE-SQS, SSE-DDB (default key)
- AWS managed keys: free, (aws/_service-name_, example: aws/rds or aws/ebs)
- customer managed keys created in KMS: $1/month
- customer managed keys imported: $1/month
- + pay for API call to KMS ($0.03/10000 calls)

## Automatic Key rotation
- AWS-managed KMS Key: automatic every 1 year
- Customer-managed KMS Key: (must be enabled) automatic & on-demand
- Imported KMS Key: only manual rotation possible using alias

## Copying Snapshots across regions
![[Copying Snapshots across regions.png]]

## Copying Snapshots across accounts
1. Create a Snapshot, encrypted with your own KMS Key (Customer Managed Key)
2. Attach a KMS Key Policy to authorize cross-account access
3. Share the encrypted snapshot
4. (in target) Create a copy of the Snapshot, encrypt it with a CMK in your account
5. Create a volume from the snapshot

## Key Policies
- Control access to KMS keys, “similar” to S3 bucket policies
- Difference: you cannot control access without them
- Default KMS Key Policy:
	- Created if you don’t provide a specific KMS Key Policy
	- Complete access to the key to the root user = entire AWS account
- Custom KMS Key Policy:
	- Define users, roles that can access the KMS key
	- Define who can administer the key
	- Useful for cross-account access of your KMS key

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
## Envelope Encryption
- KMS Encrypt API call has a limit of 4 KB
- If you want to encrypt >4 KB, we need to use Envelope Encryption
- The main API that will help us is the GenerateDataKey API
- **For the exam: anything over 4 KB of data that needs to be encrypted must use the Envelope Encryption == GenerateDataKey API**
![[GenerateDataKey API.png]]
![[Decrypt envelope data.png]]

## Encryption SDK
- The AWS Encryption SDK implemented Envelope Encryption for us
- The Encryption SDK also exists as a CLI tool we can install
- Implementations for Java, Python, C, JavaScript
- Feature - Data Key Caching:
	- re-use data keys instead of creating new ones for each encryption
	- Helps with reducing the number of calls to KMS with a security trade-off
	- Use LocalCryptoMaterialsCache (max age, max bytes, max number of messages)
- The SDK encrypts the data encryption key and stores it (encrypted) as part of the returned ciphertext.

## Request Quotas
- When you exceed a request quota, you get a ThrottlingException:
- To respond, use exponential backoff (backoff and retry)
- For cryptographic operations, they share a quota
- This includes requests made by AWS on your behalf (ex: SSE-KMS)
- For GenerateDataKey, consider using DEK caching from the Encryption SDK
- You can request a Request Quotas increase through API or AWS support

# CloudHSM
- KMS => AWS manages the software for encryption
- CloudHSM => AWS provisions encryption hardware
- Dedicated Hardware (HSM = Hardware Security Module)
- You manage your own encryption keys entirely (not AWS)
- HSM device is tamper resistant, FIPS 140-2 Level 3 compliance
- Supports both symmetric and asymmetric encryption (SSL/TLS keys)
- No free tier available
- Must use the CloudHSM Client Software
- Redshift supports CloudHSM for database encryption and key management
- Good option to use with SSE-C encryption

![[Pasted image 20241123001009.png]]
![[Pasted image 20241123001030.png]]

## CloudHSM v.s. KMS
![[Pasted image 20241123001053.png]]