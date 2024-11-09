

## Security groups
Act as a virtual firewall for your EC2 instances. Like setting up a security system for your home, deciding who can knock on the door (access your instances) and who can't.
- Another layer of abstraction over IP so that don't need to think about IPs when using groups.
- SSH

## ACL
Access Control Lists (ACLs) are used to **manage permissions at a resource level within AWS services** like S3. ACLs are not used to control access across multiple AWS accounts at the organizational level.

## Encryption
- **In flight(SSL)**
	- Data is encrypted before sending and decrypted after receiving 
	- SSL certificates help with encryption **(HTTPS)**
	- Encryption in flight ensures no MITM (man in the middle attack) can happen
- SSE at rest
	- Data is exncrypted after being received by the server, and decrypted before sent
	- The encryption / decryption keys must be managed somewhere and the server must have access to it
- CSE
	- Data is encrypted by client and never decrypted by the server but by a receiving client
	- Could leverage Envelope Encryption
	- With Client-Side Encryption, the server doesn't need to know any information about the encryption scheme being used, as the server will not perform any encryption or decryption operations.

- **Secrets Manager**
	- Newer service to store secrets(encrypted using KMS)
	- **Force rotation of secrets every X days** and automate generating secrets on rotation using **lambda**
	- **Integration with RDS(MySQL, PostgreSQL, Aurora)** and **mostly meant for RDS and Aurora integration**
	- Multi-Region Secrets
		- **Replicate Secrets across multiple AWS Regions**
		- Secrets Manager keeps read replicas in sync with the primary Secret
		- Ability to promote a read replica Secret to a standalone Secret
		- Use cases: multi-region apps, disaster recovery strategies, multi-region DB...

- **Certificate Manager(ACM)**
	- If run my app on AWS or use AWS services to serve the app, and configure ACM to validate SSL/TLS certificates for my domains, then I can use ACM's benefits: simplify certificate management, improve security, and ensure seamless HTTPS connections for users.
	- Easily provision, manage, and deploy TLS Certificates
	- Provide in-flight encryption for websites(HTTPS)
	- Support both public and private TLS certificates
	- Free of charge for public TLS certificates and automatic renewal
	- Integrate with ELB, CloudFront distributions, APIs on API Gateway. Not EC2
	- Requesting Public Certificates
		- List domain names to be included in the certificate: FQDN or Wildcard domain
		- Select Validation Method: DNS Validation or Email validation
			- DNS Validation is preferred for automation purposes  
			- Email validation will send emails to contact addresses in the WHOIS database
			- DNS Validation will leverage a CNAME record to DNS config (ex: Route 53)
		- It will take a few hours to get verified
		- The Public Certificate will be enrolled for automatic renewal
	- Importing Public Certificates: generate the certificate outside of ACM and then import it
		- No automatic renewal, must import a new certificate before expiry
		- ACM sends daily expiration events starting 45 days prior to expiration
		- AWS Config has a managed rule named `acm-certificate-expiration-check` to check for expiring certificates (configurable number of days)
	- Integration
		- with ALB: With HTTP => HTTPS redirect rule
		- with API Gateway
			- Create a Custom Domain Name in API Gateway
			- Edge-Optimized (default): For global clients
				- Requests are routed through the CloudFront Edge locations (improves latency)
				- The API Gateway still lives in only one region
				- The TLS Certificate must be in the same region as CloudFront
				- Then setup CNAME or (better) A-Alias record in Route53
			- Regional
				- For clients within the same region
				- The TLS certificate must be imported on API Gateway, in the same region as the API Stage
				- Then setup CNAME or (better) A-Alias record in Route 53
	
- WAF: Web Application Firewall
	- Protects web applications from common web exploits **(Layer 7: HTTP v.s. Lay 4 TCP/UDP)**
	- Deploy on
		- Application Load Balancer
		- API Gateway   
		- CloudFront
		- AppSync GraphQL API
		- Cognito User Pool
	- Define Web ACL (Web Access Control List) Rules. 
		- IP Set: up to 10,000 IP addresses – use multiple Rules for more IPs
		- HTTP headers, HTTP body, or URI strings Protects from common attack - SQL injection and Cross-Site Scripting (XSS)
		- Size constraints, geo-match (block countries)
		- Rate-based rules (to count occurrences of events) – for DDoS protection
	- Web ACL are Regional except for CloudFront
	- A rule group is a reusable set of rules that you can add to a web ACL
	- **WAF does not support the Network Load Balancer (Layer 4). We can use Global Accelerator for fixed IP and WAF on the ALB**
	
- **Shield: Managed Distributed Denial of Service (DDoS) protection service**
	- DDoS: Distributed Denial of Service – many requests at the same time
	- Shield standard: Free service that is activated for every AWS customer. Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer3/4 attacks.
	- Shield Advanced: Optional DDoS mitigation service ($3,000 per month per organization). Protect against more sophisticated attack on Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Route 53

- **DDoS Resiliency**
	- If we are at the edge, we can have a better DDoS protection
		- Use CloudFront or Global Accelerator, the edge-locations are auto DDoS protected.
		- Use Route 53, Domain Name Resolution at the edge also has a DDoS Protection mechanism.
	- Avoid high traffic so as to avoid DDoS
		- Infrastructure layer defense: CloudFront/Global Accelerator, Route 53, ELB
		- EC2 with Auto Scaling: in case of sudden traffic surges including a flash crowd or a DDoS attack
		- ELB: scales with traffic increases and will distribute traffic to many EC2 instances
	- Detect and filter malicious web requests (BP1, BP2) + Shield Advanced (BP1, 2, 6)
	- Obfuscating AWS resources(BP1, BP4, BP6)
		- Using CloudFront, API Gateway, Elastic Load Balancing to hide your backend resources (Lambda functions, EC2 instances)
	- ![[Best-practice-for-DDoS-resiliency attach-surface-reduction.png]]

- **Firewall Manager**
	- A security management service that **centrally configure and manage AWS WAF rules across multiple AWS accounts and resources**. But is not specifically designed for traffic inspection and filtering within a single VPC.
	- Rules are applied to new resources as they are created (good for compliance) across all and future accounts in your Organization
	- **v.s. Network FireWall: AWS Network Firewall focuses on providing network traffic filtering and protection for individual VPCs, Firewall Manager is designed for centralized management and enforcement of firewall policies across multiple AWS accounts and resources, including Network Firewall.**

![[WAF-Firewall-manager-Shield.png]]

- **GuardDuty**
	- A threat detection service that continuously monitors for **malicious activity and unauthorized behavior in AWS accounts and workloads**. It is not specifically designed for traffic inspection or filtering within a VPC. It focuses on identifying threats and anomalies based on network traffic, AWS API calls, and DNS data.
	- Input data includes:  
		- CloudTrail Events Logs – unusual API calls, unauthorized deployments
		- CloudTrailManagementEvents–create VPC subnet, create trail,...
		- CloudTrailS3DataEvents–get object, list objects, delete object,...
		- **VPC Flow Logs** – unusual internal traffic, unusual IP address  
		- **DNS Logs** – compromised EC2 instances sending encoded data within DNS queries
		- Optional Features – EKS Audit Logs, RDS & Aurora, EBS, Lambda, S3 Data Events...
	- Then generate findings.
	- Can setup EventBridge rules to be notified in case of findings
	- Can protect against CryptoCurrency attacks (has a dedicated “finding” for it)
	
- Inspector: Automated Security Assessment **only for EC2, Container Images and Lambda**
	- For EC2 instances  
		- Leveraging the AWS System Manager (SSM) agent  
		- Analyze against unintended network accessibility  
		- Analyze the running OS against known vulnerabilities
	- For Container Images push to Amazon ECR
		- Assessment of Container Images as they are pushed
	- For Lambda Functions
		- Identifies software vulnerabilities in function code and package dependencies
		- Assessment of functions as they are deployed
	- Reporting & Integration with AWS Security Hub
	- **Send findings to Amazon Event Bridge**

- Macia
	- A fully managed data security and data privacy service that **uses machine learning and pattern matching to discover and protect sensitive data** in AWS.
	- A security service to help organizations discover, classify, and protect sensitive data stored in AWS. It uses machine learning and pattern matching techniques to automatically identify and classify sensitive data such as personally identifiable information (PII), intellectual property, and financial data. Macie can analyze data stored in various AWS services, including S3 buckets, Amazon RDS databases, and AWS Glue Data Catalogs.
	- Identify and alert you to sensitive data, such as personally identifiable information (PII)



## Data Security

### AWS Secrets Manager
storing and managing sensitive information such as database credentials.
specifica
lly designed for managing secrets like database credentials. It provides additional features tailored for secret management, such as integration with RDS for automatic rotation, and it's generally considered the best practice for managing credentials securely.

### AWS Systems Manager Parameter Store
storing and managing sensitive information such as database credentials.
a versatile service primarily used for storing configuration data and secrets. While it does support automatic rotation of parameters, it's not specifically tailored for secret management like AWS Secrets Manager. Parameter Store may be suitable for managing other types of configuration data but may lack some of the advanced features and integrations provided by Secrets Manager for managing secrets like database credentials.





